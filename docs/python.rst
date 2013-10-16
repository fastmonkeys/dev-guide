Python
======

Coding standards
----------------

- `Hitchhiker's guide to Python`_


Flask
-----



Internationalization
--------------------

* Babel_
* `Flask-Babel`_
* `SQLAlchemy-i18n`_


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

- Keep things as close to database as possible. For example calculate data aggregates on the database side rather than on the Python side.

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


Testing with pytest
-------------------


Running single test
*******************

::


    $ py.test tests/test_something.py -k some_test_method


Using debugger
**************

Whenever you have a failing test case you should use `Python Debugger`_.



.. _`Hitchhiker's guide to Python`: http://docs.python-guide.org/en/latest/
.. _`full text search`: http://en.wikipedia.org/wiki/Full_text_search
.. _`Babel`: http://babel.pocoo.org/
.. _`Flask-Babel`: http://pythonhosted.org/Flask-Babel/
.. _`SQLAlchemy-i18n`: https://sqlalchemy-i18n.readthedocs.org/en/latest/
.. _`SQLAlchemy-Searchable`: https://sqlalchemy-searchable.readthedocs.org/en/latest/
.. _`SQLAlchemy-Continuum`: https://sqlalchemy-continuum.readthedocs.org/en/latest/
.. _`deferred column loading`: http://docs.sqlalchemy.org/en/latest/orm/mapper_config.html#deferred-column-loading
.. _`Python Debugger`: http://pytest.org/latest/usage.html#dropping-to-pdb-python-debugger-on-failures
