Settings and Configs
====================

There is a single ``settings.py`` file located in your project directory.

.. code-block:: text

   myproject
   ├── myproject
   │  ├── settings.py
   │  ...

As suggested in the `Twelve-Factor App <https://12factor.net/config>`_ methodology, the settings values are pulled from environment variables
using `environs <https://github.com/sloria/environs>`_. Most settings are configured with default values or are made optional so that the project can be easily set up in development, production, or even a staging environment.
The settings are organized following recommendations from `Boost Your Django DX <https://adamchainz.gumroad.com/l/byddx>`_.

The ``DEBUG`` value is use to distinguish between development and production environments.

- ``DEBUG=True`` means development
- ``DEBUG=False`` means production

.. note::
    The ``PROD`` alias is defined as ``not DEBUG`` to simplify reading the settings.

You won't even be able to set ``DEBUG=True`` in production since the development requirements will be missing. They are not included in the provided methods of building the project for production.


Database
--------

The project is configured to use `sqlite <https://www.sqlite.org/index.html>`_ by default for everything, development, production, cache and background tasks, but you can easily change it up to
`postgresql <https://www.postgresql.org/>`_, the dependencies for it are already installed.
`django-litestream <https://github.com/Tobi-De/django-litestream>`_ is included for to use a backup mechanism to sqlite, to disable it you need to set:

.. code-block:: shell

    USE_LITESTREAM=False


There is no specific setup done for the database, you deal with this how you want. If you go with a managed solution like `aiven <https://aiven.io/postgresql>`_ they usually provide a backup solution,
it you also go with a PaaS to host your plateform fo your solution they usually provide a database service with automatic backup.

If you are using postgresql in production, a simple solution is to use `django-db-backup <https://github.com/jazzband/django-dbbackup>`_ with ``django-q2`` for the task that automatically backups, if you are using sqlite
I recommend .
I've being ridin myserlf the sqlite bandwagon recently and for my personal projects my go to has being ``sqlite + litestream``, I even have a branch for this on the `tailwind falco blueprint <ttps://github.com/Tobi-De/falco_tailwind/pull/67>`_.
This is evenetually going come to come to the default falco setup.

Cache
-----

The cache backend is configure to use `diskcache <https://github.com/grantjenks/python-diskcache>`_.

    DiskCache is an Apache2 licensed disk and file backed cache library, written in pure-Python, and compatible with Django.

    -- diskcache github page

By default it's not enable, all you have to do to enable it is to add the ``LOCATION`` environment variable with the path to the cache directory.

.. tabs::

    .. tab:: settings.py

        .. literalinclude:: ../{{ cookiecutter.project_name }}/{{ cookiecutter.project_name }}/settings.py
            :linenos:
            :lineno-start: 41
            :lines: 41-55

    .. tab:: Environment variable

        .. code-block:: bash
            :caption: Example

            CACHE_LOCATION=.diskcache

If you are running in docker, you can tadd a volume to the container to persist the cache files.

Media files
-----------

By default media files are configured to be be stored using the filesystemd storage, so by default they will be saved at whatever location is defined by ``MEDIA_ROOT``
and the value of this settings is configurable via  environment variable with the same name.
`django-storages <https://github.com/jschneier/django-storages>`_ is also include  in the project, with the default configuration set to use `s3 <https://aws.amazon.com/s3/>`_.
This is what I personally use, to make use of this instead of the filesystem storage you need to set the environment variables below

.. tabs::

    .. tab:: Environment variables

        .. code-block:: bash
            :caption: Example

            USE_S3=True
            AWS_ACCESS_KEY_ID=your_access_key
            AWS_SECRET_ACCESS_KEY=your_secret_key
            AWS_STORAGE_BUCKET_NAME=your_bucket_name
            AWS_S3_REGION_NAME=your_region_name

    .. tab:: settings.py ( media root )

        .. literalinclude:: ../{{ cookiecutter.project_name }}/{{ cookiecutter.project_name }}/settings.py
            :linenos:
            :lineno-start: 188
            :lines: 188-188

    .. tab:: settings.py ( storage backend )

        .. literalinclude:: ../{{ cookiecutter.project_name }}/{{ cookiecutter.project_name }}/settings.py
            :linenos:
            :lineno-start: 247
            :lines: 247-256

This `guide <https://testdriven.io/blog/storing-django-static-and-media-files-on-amazon-s3/>`_ is an excellent resource to help you setup an s3 bucket for your media files.

Emails
------


`django-anymail <https://anymail.dev/en/stable/>`_ is what is used by the project for emails sending, they support a lot of emails, provider, by default the project
is configured to use `ses <https://aws.amazon.com/ses/>`_. The same environmments variables

.. tabs::

    .. tab:: Email Backend

        .. literalinclude:: ../{{ cookiecutter.project_name }}/{{ cookiecutter.project_name }}/settings.py
            :linenos:
            :lineno-start: 89
            :lines: 89-93

    .. tab:: Anymail config

        .. literalinclude:: ../{{ cookiecutter.project_name }}/{{ cookiecutter.project_name }}/settings.py
            :linenos:
            :lineno-start: 369
            :lines: 369-376

It uses the environments variables so the same keys as for media files, this might be considered bad practice by some, feel free to change them.


