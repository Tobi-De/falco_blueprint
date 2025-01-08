Getting Started
===============

Here is a quick guide to get you started with a new falco project.

Create a new project
--------------------

First, make sure you have `uv <https://docs.astral.sh/uv/>`_ and `just <https://just.systems/>`_ installed on your system.

.. admonition:: Install just with uv
    :class: tip dropdown

    If you are a Linux or macOS user, you can install ``just`` by running :

    .. code-block:: shell

        uv tool install just-bin

    For Windows users, I highly suggest using `WSL2 <https://learn.microsoft.com/en-us/windows/wsl/install>`_ if possible, and running the project in a Linux environment. If that's not feasible, follow the official `just installation guide <https://just.systems/man/en/packages.html#windows>`_.

You have two ways to create a new falco project:

The CLI
~~~~~~~

Install the `falco-cli <https://github.com/falcopackages/falco-cli>`_ by running the following command:

.. code-block:: shell

    uv tool install falco-cli

Then run one of the following commands to create a new project:

.. code-block:: shell
    :caption: tailwindcss

    falco new myproject

.. code-block:: shell
    :caption: bootstrap5

    falco new myproject -f bs5

Read the `CLI documentation <https://github.com/falcopackages/falco-cli>`_ for all available options and commands.

GitHub Template Repository
~~~~~~~~~~~~~~~~~~~~~~~~~~

You can also create a new project directly on GitHub using the `template repository <https://github.com/falcopackages/falco-template-repository>`_.
Unlike most template repositories, using it as a template won’t automatically get you a new falco project. Instead, when your repo is created,
a one-time GitHub action runs to generate the project content and push it to your repository.

.. button-link:: https://github.com/falcopackages/falco-template-repository/generate
    :color: primary
    :outline:

    Click here to create a new project

Run project setup
-----------------

Navigate to your project directory and run:

.. code-block:: shell

    just setup

This will:

- Initialize a new git repository.
- Rename the GitHub workflows directory (only relevant if you created the project from the GitHub template).
- Download Python (if needed), create a virtual environment, and install dependencies.
- Install pre-commit hooks.
- Run migrations.
- Create a superuser for development with the credentials: email ``admin@localhost`` and password ``admin``.
- Run code formatters.
- Create a new ``.env`` file with the ``DEBUG`` variable set to ``True``.

As with a typical Django project, the default development database is ``sqlite``. However, this project rocks hard on sqlite,
including production-ready configurations, using it not only as the main database but also for caching and storing background tasks.
The upcoming sections will provide more details on how all this works.

Run the project
---------------

At this point, you can run the project by executing:

.. code-block:: shell

    just server

This will start Django's ``runserver``, the TailwindCSS watch process (if using Tailwind), and Django’s ``db_worker`` for background tasks.
Your project will be running at `http://localhost:8000 <http://localhost:8000>`_.
You’ll see the following landing page:

.. image:: /_static/img/landing.png
    :alt: Landing Page

At this point, you are ready to start hacking on your project—customize the landing page, create a new Django application, and start writing
your models.

Next Up
-------

.. grid:: 1 1 1 2
    :class-row: surface
    :gutter: 2
    :padding: 0

    .. grid-item-card:: :octicon:`rocket` Deploy
      :link: deploy/index
      :link-type: doc

      Check out the deployment guide for instructions on deploying your project.

    .. grid-item-card:: :octicon:`package` Toolbox
      :link: https://falco-app.readthedocs.com/guides

      Check out the **falco app** for available helpers like the ``start_app`` and ``crud`` commands.
