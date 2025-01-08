Entry point and Binary
======================

There is a `__main__.py <https://docs.python.org/3/library/__main__.html#main-py-in-python-packages>`_ file inside your project directory, next to your ``settings.py`` file.
This is the main entry point of your app. This is what the binary app built with `pyapp <https://github.com/ofek/pyapp>`_ uses. Commands run inside the Docker container also use this file.
This file effectively replace your ``manage.py`` file, as it contains the same content, which is why you won't find a ``manage.py`` file in the project.

.. admonition:: More on this binary file thing
   :class: note dropdown

   The binary file that ``pyapp`` builds is a script that bootstraps itself the first time it is run, meaning it will create its own isolated virtual environment with **its own Python interpreter**.
   It installs the project (your falco project is setup as a python package) and its dependencies. When the binary is built, either via the provided GitHub Action or the ``just`` recipe / command,
   you also get a wheel file (the standard format for Python packages). If you publish that wheel file on PyPI, you can use the binary's ``self update`` command to update the project.

Let's assume you generated a project with the name ``myproject``:

.. code-block:: shell
   :caption: Example of how to invoke the script

   just run python myproject/__main__.py
   just run python -m myproject
   just run myproject

All the commands above do exactly the same thing.

.. code-block:: shell
   :caption: Usage Example

   just run myproject # Prints all available django commands
   just run myproject db_worker # Runs the django-tasks database worker for background tasks
   just run myproject setup # Runs the custom setup command
   just run myproject runserver # Runs the django dev server
   just run myproject dbshell # Opens the dbshell

The binary is automatically built on every new push via the GitHub Action in the ``.github/workflows/cd.yml`` file. You can also build it locally by running the following commands:

.. code-block:: shell
   :caption: Building the binary

   just build-bin # Builds for the current platform and architecture (e.g., if you are on an Intel macOS, it will build for macOS x86_64)
   just build-linux-bin # Always builds for Linux x86_64

For more details on deploying the binary to a VPS, check out the `deployment guide </deploy.html>`_.