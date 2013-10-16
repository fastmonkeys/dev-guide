Python
======

Coding standards
----------------

- `Hitchhiker's guide to Python`_

Flask
-----


SQLAlchemy
----------

Perfomance tuning
*****************

- Index all foreign keys

- Never use LIKE '%keyword%' for searching, use full text indexes instead.

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
.. _`deferred column loading`: http://docs.sqlalchemy.org/en/latest/orm/mapper_config.html#deferred-column-loading
