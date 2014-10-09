Databases
=========


PostgreSQL
----------

Installation
************

`Postgres.app`_: "The easiest way to get started with PostgreSQL on the Mac"

Useful commands
***************

Open database console

::


    $ psql <database-name>


List all tables

::


    mydatabase=# \d


List columns of a table

::


    mydatabase=# \d <table>



Use EXISTS instead of IN
************************


http://ioanadanes.blogspot.fi/2011/07/postgres-90-exists-vs-in.html


Things to remember
******************


- Unlike some other databases, PostgreSQL_ does *NOT* index foreign keys by default. You should always remember to index all foreign keys.

The following example using Python and SQLAlchemy. Do NOT do this:


::


    class Article(Base):
        id = sa.Column(sa.Integer, autoincrement=True)
        name = sa.Column(sa.Unicode(255))
        author_id = sa.Column(sa.Integer, sa.ForeignKey(User.id))


Instead do this:


::


    class Article(Base):
        id = sa.Column(sa.Integer, autoincrement=True)
        name = sa.Column(sa.Unicode(255))
        author_id = sa.Column(sa.Integer, sa.ForeignKey(User.id), index=True)


Redis
-----

Installation
************

Via Homebrew:

::

    $ brew install redis


Useful commands
***************

Ping Redis server

::

    redis-cli ping

Clear Redis database

::

    redis-cli flushdb


.. _`Postgres.app`: http://postgresapp.com/
.. _PostgreSQL: http://www.postgresql.org/
