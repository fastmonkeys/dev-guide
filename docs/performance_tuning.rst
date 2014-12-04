Performance tuning
==================

Consider the following model structure:

- User
 - id
 - email

- Movie
 - id
 - name
 - year
 - director_id
 - _director_name
 - director
 - genres
 - ratings
 - avg_rating


- Director
 - id
 - name

- MovieGenre
 - movie_id
 - genre_id

- Genre
 - id
 - name


- Rating
 - user_id
 - movie_id
 - stars


Naive way of doing things
-------------------------

DO NOT DO THIS:

::

    from statistics import mean

    for movie in Movie.query.all()[:100]:
        print (
            movie.id,
            movie.name,
            movie.director.name,  # Executes query per iteration
            [genre.name for genre in movie.genres],
            mean(
                rating.stars
                for rating in movie.ratings
            )
        )


Add proper indexes
------------------

* If you order by name always remember to have compound index on both name
and table primary key.

::

    name = db.Column(sa.String, index=True)  # Don't use this

    __table_args__ = (  # Use this!
        db.Index('movie_name_id_idx', Movie.name, Movie.id)
    )


Joinedloading
-------------

::


    query = Movie.query.options(
        sa.orm.joinedload(Movie.director)
    ).limit(100)


SELECT ... FROM movie JOIN director ON ...


Subqueryloading
---------------


* Use subqueryloading only if the query result is deterministic. Query
result is deterministic if the query is ordered by a unique column.

::

    query = Movie.query.options(
        sa.orm.subqueryload(Movie.director)
    ).limit(100) # This is WRONG! Leads to errors.


SELECT ... FROM movie

SELECT ... FROM (SELECT ... FROM movie) director


::

    query = Movie.query.options(
        sa.orm.subqueryload(Movie.director)
    ).order_by(Movie.name, Movie.id).limit(100)


Batch fetching
--------------

* Useful for loading one-to-one relationships

::

    movies = Movie.query.limit(100).all()

    directors = Director.query.filter(
        Director.id.in_([movie.director_id for movie in movies])
    )

    for movie in movies:
        # ...
        pass

Denormalization
---------------

* Have a compound foreign key from (_director_name, director_id) to
(Director.name, Director.id)

::


    for movie in movies:
        print movie.id, movie.name, movie.director_name


Column properties
-----------------

::

    Movie.avg_rating = db.column_property(
        db.select([db.func.avg(Rating.stars)])
        .where(Rating.movie_id == Movie.id)
        .correlate(Movie)
        .label('avg_rating')
    )

    Movie.query

    print db.session.query(Movie.avg_rating)



SELECT
    id,
    name,
    (
        SELECT AVG(rating.stars)
        FROM rating
        WHERE rating.movie_id=movie.id
    ) AS avg_rating
FROM movie


Hybrid properties
-----------------


Precalculation
--------------


* Aggregated only works on ORM level. So If you are updating database
directly then aggregated won't help.

::


    from sqlalchemy_utils import aggregated


    class Movie(db.Model):
        ...

        @aggregated(
            'ratings', db.Column(db.Integer)
        )
        def avg_rating(self):
            return db.func.avg(Rating.stars)


UPDATE movie SET avg_rating = (SELECT ... )


* The fastest way of precalculation is incremental updates with database
triggers. Triggers are problematic in many ways though. For example you have to
remember which triggers reference different columns when doing schema migrations.

Use load_only
-------------

* Many times when you are fetching objects you don't need all the columns. Use
load_only to fetch only the columns you need.

::

    movies = Movie.query.options(db.load_only(Movie.name)).all()

    movies[0].id  # executes a query

Returning results in KeyedTuples rather than objects
----------------------------------------------------

Bad

::

    [movie.name for movie in Movie.query]

Better

::

    [movie.name for movie in Movie.query.options(db.load_only(Movie.name))]

Best

::

    [name for (name,) in db.session.query(Movie.name)]

Not rendering templates in server side
--------------------------------------


Always precalculate search vectors
----------------------------------

::

    (movie.search_vector || director.search_vector) @@ to_tsquery('keyword:*')

    movie.fat_search_vector
    # pre calculated search vector that contains both movie.search_vector
    # and director.search_vector


    movie.director_search_vector



Return JSON directly from PostgreSQL
------------------------------------

* SELECT row_to_json(m.*) FROM (SELECT * FROM movie) AS m


    db.select([db.func.row_to_json(...)])
