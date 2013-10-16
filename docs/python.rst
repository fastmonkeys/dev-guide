Python
======

Coding standards
----------------

- `Hitchhiker's guide to Python`_

Flask
-----


SQLAlchemy
----------

Versioning
**********

Use `SQLAlchemy-Continuum`_ whenever project needs data versioning.


Searching
*********

Use `SQLAlchemy-Searchable`_ whenever project needs `full text search`_ capabilities.


Perfomance tuning
*****************

- Index all foreign keys

- Calculate things as close to database as possible. For example calculate data aggregates on the database side rather than Python side.

Example 1. Count all articles

Do *NOT* do this:


::

    len(Article.query)


Instead do this:


::

    Article.query.count()



- Never use LIKE '%keyword%' for searching, use `SQLAlchemy-Searchable`_  and full text indexes instead.

- Load only the columns you need using `deferred column loading`_.

- Load one-to-one and many-to-one data at once. Load one-to-many and many-to-many relation data using subqueryload or batch_fetch.

Subqueryloading all articles and associated tags:

::

    articles = Article.query.options(db.subqueryload(Article.tags))


Batch fetching all articles and associated tags:

::

    from sqlalchemy_utils import batch_fetch


    articles = Article.query.all()

    batch_fetch(articles, ['tags'])


.. _`Hitchhiker's guide to Python`: http://docs.python-guide.org/en/latest/
.. _`full text search`: http://en.wikipedia.org/wiki/Full_text_search
.. _`SQLAlchemy-Searchable`: https://sqlalchemy-searchable.readthedocs.org/en/latest/
.. _`SQLAlchemy-Continuum`: https://sqlalchemy-continuum.readthedocs.org/en/latest/
.. _`deferred column loading`: http://docs.sqlalchemy.org/en/latest/orm/mapper_config.html#deferred-column-loading

