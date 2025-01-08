Custom Commands
===============

.. code-block:: text

   myproject
   ├── myproject
   │──  ├── management
   │──  │──  ├── commands
   │──  │──  │──  ├── __init__.py
   │──  │──  │──  ├── prodserver.py
   │──  │──  │──  └── setup.py


The project comes with two custom Django commands:

prodserver
----------

Runs the production server, currently `granian <https://github.com/emmett-framework/granian>`_. If the ``use_litestream`` function returns ``True``, it will also run the Litestream replication command.

.. note::

    The source code of the ``use_litestream`` function is in the command source file. It returns ``True`` if the current database is SQLite and the ``USE_LITESTREAM`` environment variable is ``True``, which is the default.

setup
-----

Runs one-off setup tasks when the app is deployed, including:

- Generate the Litestream config file (if ``use_litestream`` returns ``True``)
- Restore the SQLite database if it does not exist locally and a backup exists (if ``use_litestream`` returns ``True``)
- Run migrations for the main database
- Run migrations for the tasks database
- Create a superuser from the environment variables ``DJANGO_SUPERUSER_EMAIL`` and ``DJANGO_SUPERUSER_PASSWORD``