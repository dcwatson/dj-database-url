DJ-Database-URL
~~~~~~~~~~~~~~~

.. image:: https://jazzband.co/static/img/badge.png
   :target: https://jazzband.co/
   :alt: Jazzband

.. image:: https://github.com/jazzband/dj-database-url/actions/workflows/test.yml/badge.svg
   :target: https://github.com/jazzband/dj-database-url/actions/workflows/test.yml

.. image:: https://codecov.io/gh/jazzband/dj-database-url/branch/master/graph/badge.svg?token=7srBUpszOa
   :target: https://codecov.io/gh/jazzband/dj-database-url

This simple Django utility allows you to utilize the
`12factor <http://www.12factor.net/backing-services>`_ inspired
``DATABASE_URL`` environment variable to configure your Django application.

The ``dj_database_url.config`` method returns a Django database connection
dictionary, populated with all the data specified in your URL. There is
also a `conn_max_age` argument to easily enable Django's connection pool.

If you'd rather not use an environment variable, you can pass a URL in directly
instead to ``dj_database_url.parse``.

Supported Databases
-------------------

Support currently exists for PostgreSQL, PostGIS, MySQL, MySQL (GIS),
Oracle, Oracle (GIS), Redshift, CockroachDB, and SQLite.

Installation
------------

Installation is simple::

    $ pip install dj-database-url

Usage
-----

1. If ``DATABASES`` is already defined:

- Configure your database in ``settings.py`` from ``DATABASE_URL``::

    import dj_database_url

    DATABASES['default'] = dj_database_url.config(conn_max_age=600)

- Provide a default::

    DATABASES['default'] = dj_database_url.config(default='postgres://...')

- Parse an arbitrary Database URL::

    DATABASES['default'] = dj_database_url.parse('postgres://...', conn_max_age=600)

2. If ``DATABASES`` is not defined:

- Configure your database in ``settings.py`` from ``DATABASE_URL``::

    import dj_database_url

    DATABASES = {'default': dj_database_url.config(conn_max_age=600)}

- Provide a default::

    DATABASES = {'default': dj_database_url.config(default='postgres://...')}

- Parse an arbitrary Database URL::

    DATABASES = {'default': dj_database_url.parse('postgres://...', conn_max_age=600)}

The ``conn_max_age`` attribute is the lifetime of a database connection in seconds
and is available in Django 1.6+. If you do not set a value, it will default to ``0``
which is Django's historical behavior of using a new database connection on each
request. Use ``None`` for unlimited persistent connections.

Strings passed to `dj_database_url` must be valid URLs; in
particular, special characters must be url-encoded. The following url will raise
a `ValueError`::
    postgres://user:p#ssword!@localhost/foobar
and should instead be passed as::
    postgres://user:p%23ssword!@localhost/foobar

URL schema
----------

+-------------+-----------------------------------------------+--------------------------------------------------+
| Engine      | Django Backend                                | URL                                              |
+=============+===============================================+==================================================+
| PostgreSQL  | ``django.db.backends.postgresql`` [1]_        | ``postgres://USER:PASSWORD@HOST:PORT/NAME`` [2]_ |
+-------------+-----------------------------------------------+--------------------------------------------------+
| PostGIS     | ``django.contrib.gis.db.backends.postgis``    | ``postgis://USER:PASSWORD@HOST:PORT/NAME``       |
+-------------+-----------------------------------------------+--------------------------------------------------+
| MSSQL       | ``sql_server.pyodbc``                         | ``mssql://USER:PASSWORD@HOST:PORT/NAME``         |
+-------------+-----------------------------------------------+--------------------------------------------------+
| MSSQL [5]_  | ``mssql``                                     | ``mssqlms://USER:PASSWORD@HOST:PORT/NAME``       |
+-------------+-----------------------------------------------+--------------------------------------------------+
| MySQL       | ``django.db.backends.mysql``                  | ``mysql://USER:PASSWORD@HOST:PORT/NAME``         |
+-------------+-----------------------------------------------+--------------------------------------------------+
| MySQL (GIS) | ``django.contrib.gis.db.backends.mysql``      | ``mysqlgis://USER:PASSWORD@HOST:PORT/NAME``      |
+-------------+-----------------------------------------------+--------------------------------------------------+
| SQLite      | ``django.db.backends.sqlite3``                | ``sqlite:///PATH`` [3]_                          |
+-------------+-----------------------------------------------+--------------------------------------------------+
| SpatiaLite  | ``django.contrib.gis.db.backends.spatialite`` | ``spatialite:///PATH`` [3]_                      |
+-------------+-----------------------------------------------+--------------------------------------------------+
| Oracle      | ``django.db.backends.oracle``                 | ``oracle://USER:PASSWORD@HOST:PORT/NAME`` [4]_   |
+-------------+-----------------------------------------------+--------------------------------------------------+
| Oracle (GIS)| ``django.contrib.gis.db.backends.oracle``     | ``oraclegis://USER:PASSWORD@HOST:PORT/NAME``     |
+-------------+-----------------------------------------------+--------------------------------------------------+
| Redshift    | ``django_redshift_backend``                   | ``redshift://USER:PASSWORD@HOST:PORT/NAME``      |
+-------------+-----------------------------------------------+--------------------------------------------------+
| CockroachDB | ``django_cockroachdb``                        | ``cockroach://USER:PASSWORD@HOST:PORT/NAME``     |
+-------------+-----------------------------------------------+--------------------------------------------------+

.. [1] The django.db.backends.postgresql backend is named django.db.backends.postgresql_psycopg2 in older releases. For
       backwards compatibility, the old name still works in newer versions. (The new name does not work in older versions).
.. [2] With PostgreSQL, you can also use unix domain socket paths with
       `percent encoding <http://www.postgresql.org/docs/9.2/interactive/libpq-connect.html#AEN38162>`_:
       ``postgres://%2Fvar%2Flib%2Fpostgresql/dbname``.
.. [3] SQLite connects to file based databases. The same URL format is used, omitting
       the hostname, and using the "file" portion as the filename of the database.
       This has the effect of four slashes being present for an absolute file path:
       ``sqlite:////full/path/to/your/database/file.sqlite``.
.. [4] Note that when connecting to Oracle the URL isn't in the form you may know
       from using other Oracle tools (like SQLPlus) i.e. user and password are separated
       by ``:`` not by ``/``. Also you can omit ``HOST`` and ``PORT``
       and provide a full DSN string or TNS name in ``NAME`` part.
.. [5] Microsoft official `mssql-django <https://github.com/microsoft/mssql-django>`_ adapter.

Contributing
------------

We welcome contributions to this project. Projects can take two forms:

1. Raising issues or helping others through the github issue tracker.
2. Contributing code.

Raising Issues or helping others:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

When submitting an issue or helping other remember you are talking to humans who have feelings, jobs and lives of their
own. Be nice, be kind, be polite. Remember english may not be someone first language, if you do not understand or
something is not clear be polite and re-ask/ re-word.

Contributing code:
^^^^^^^^^^^^^^^^^^

* Before writing code be sure to check existing PR's and issues in the tracker.
* Write code to the pylint spec.
* Large or wide sweeping changes will take longer, and may face more scrutiny than smaller confined changes.
* Code should be pass `black` and `flake8` validation.
