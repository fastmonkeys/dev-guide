Python
======


PIP
---

Updating package wheel
**********************

When updating a package wheel you need to the following things:

1. Generate the wheel file (the example uses SQLAlchemy-Continuum package with version 0.10.0)

::


    $ pip wheel --wheel-dir=./wheels SQLAlchemy-Continuum==0.10.0


2. Check which dependant wheel files were affected by running git status and checking which wheel files were modified:


::

    $ git status


3. Run git checkout for each of those files (we don't want to bloat git version history)

::


    $ git checkout xxx.wheel



4. Remove the old wheel file of this package, for example:


::


    $ git rm SQLAlchemy_Continuum-0.8.5-py27-none-any.whl




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

Do **NOT** do this:


::

    len(Article.query)


Instead do this:


::

    Article.query.count()



- Never use LIKE '%keyword%' for searching, use `SQLAlchemy-Searchable`_  and full text indexes instead.

- Load only the columns you need using `deferred column loading`_.

- For long running data manipulation operations use background processing.

- Load one-to-one and many-to-one data at once. Load one-to-many and many-to-many relation data using subqueryload or batch_fetch.

Subqueryloading all articles and associated tags:

::

    articles = Article.query.options(db.subqueryload(Article.tags))


Batch fetching all articles and associated tags:

::

    from sqlalchemy_utils import batch_fetch


    articles = Article.query.all()

    batch_fetch(articles, ['tags'])


Alembic
-------


List revisions
**************

::


    $ alembic history


Autogenerating new revision
***************************

::


    $ alembic revision --autogenerate





Testing with pytest
-------------------


Coding standards
****************

- Write small test methods presumably with one assert per test method
- Test method name should correlate with the testing scenario

Bad naming:


::

    def test_save():
        pass


    def test_save2():
        pass


Better naming:


    def test_save_returns_true_on_s()
        pass

    def test_save_trhows_exception_on_failure(self)
- Reset the test case state after each method call (in order to avoid memory leaking and to make tests isolated from each other)


Running single test
*******************

::


    $ py.test tests/test_something.py -k some_test_method


Using debugger
**************

Whenever you have a failing test case you should use `Python Debugger`_.




Functional programming
----------------------

https://github.com/Suor/funcy
https://github.com/kachayev/fn.py




Design patterns
---------------

http://martinfowler.com/eaaCatalog/




Refactoring
-----------

http://hackflow.com/blog/2013/10/08/abstracting-control-flow/



Misc.
-----

Chartkick.py
************
https://github.com/mher/chartkick.py

Create beautiful Javascript charts with minimal code. Supports Google Charts and Highcharts, works with Flask/Jinja2.

.. _`Hitchhiker's guide to Python`: http://docs.python-guide.org/en/latest/
.. _`full text search`: http://en.wikipedia.org/wiki/Full_text_search
.. _`Babel`: http://babel.pocoo.org/
.. _`Flask-Babel`: http://pythonhosted.org/Flask-Babel/
.. _`SQLAlchemy-i18n`: https://sqlalchemy-i18n.readthedocs.org/en/latest/
.. _`SQLAlchemy-Searchable`: https://sqlalchemy-searchable.readthedocs.org/en/latest/
.. _`SQLAlchemy-Continuum`: https://sqlalchemy-continuum.readthedocs.org/en/latest/
.. _`deferred column loading`: http://docs.sqlalchemy.org/en/latest/orm/mapper_config.html#deferred-column-loading
.. _`Python Debugger`: http://pytest.org/latest/usage.html#dropping-to-pdb-python-debugger-on-failures
