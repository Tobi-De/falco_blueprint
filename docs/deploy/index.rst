:description: How to deploy a project generated with falco.

Deployment
==========

.. warning::

    Work in progress. To receive updates follow me on `x <https://twitter.com/tobidegnon>`_ or `mastodon <https://fosstodon.org/@tobide>`_.

There are two options configured option to deploy project generated with falco, the first option is centered around the traditional vps stack deployment but with a lot of tools to automate the process, the second option
is centered around deploying using docker, this should also work with any PAAS service that support docker like caprover, coolify, digital ocean app platform, fly.io, heroku, render, etc.


Environment Variables
*********************

These are te mininum required environment variables to make your project run:

.. code-block:: bash

    SECRET_KEY=your_secret_key
    ALLOWED_HOSTS=your_host
    DATABASE_URL=your_database_url
    ADMIN_URL=your_admin_url # not required but recommended
    USE_S3=False # disable use of s3 for media files
    USE_LITESTREAM=False # disable backup with litestream if you are using sqlite






.. toctree::
   :hidden:

   fujin
   docker