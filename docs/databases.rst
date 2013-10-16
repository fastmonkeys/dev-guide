Databases
=========


PostgreSQL
----------

Things to remember
^^^^^^^^^^^^^^^^^^

- Unlike some other databases, PostgreSQL does *NOT* index foreign keys by default. You should always remember to index all foreign keys.

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


