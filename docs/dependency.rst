:description: Virtualenv and dependencies management with a project generated with falco.

Python Environment
==================

This is mainly handled using `uv <https://docs.astral.sh/uv/>`_ and the ``pyproject.toml`` file.

pyproject.toml
--------------

The ``pyproject.toml`` file is a Python standard introduced to unify and simplify Python project packaging and configurations. It was introduced by `PEP 518 <https://www.python.org/dev/peps/pep-0518/>`_ and `PEP 621 <https://www.python.org/dev/peps/pep-0621/>`_.
For more details, check out the `complete specifications <https://packaging.python.org/en/latest/specifications/pyproject-toml/#pyproject-toml-spec>`_.
Many tools in the Python ecosystem, including ``uv``, support it. You'll find configurations for most included tools related to linting, formatting, testing, etc., and your project metadata in this file.

uv
--

`UV <https://docs.astral.sh/uv/>`_ is the main requirement you need on your system to run falco projects. Once you get it `installed <https://docs.astral.sh/uv/getting-started/installation/>`_, you are ready to go. There is a docker setup
included with the project, but it's meant for deployment in production. Most commands you need are hidden behind the ``just`` command.

.. note::
    If you are not familiar with ``uv``, reading their `working on projects guide <https://docs.astral.sh/uv/guides/projects/>`_ should give you a good overview
    of how to use it.

For the context of falco projects, this is what you need to know:

.. code-block:: shell

    just bootstrap # download python (if needed), create venv, install dependencies, this command is run when you run just setup
    uv add django-tomselect # add a new base dependency to the project
    uv add --dev django-functest # add a new development dependency to the project
    just lock # generate a requirements file, but there is also a pre-commit hook for this
    just upgrade # upgrade the dependencies to the latest version
    just clean # remove the venv and clean some project cache

.. tip::

    Run ``just`` to see all the available commands you can run with the project.
