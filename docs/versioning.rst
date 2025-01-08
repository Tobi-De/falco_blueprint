Project versioning
==================

It is always a good idea to keep a versioning system in place for your project. The project includes the following tools to make the process as simple and low maintenance as possible:

- `git-cliff <https://git-cliff.org/>`_: Generate changelog for your project based on your commit messages, provided they follow the `conventional commits <https://www.conventionalcommits.org/en/v1.0.0/>`_ format.
- `bump-my-version <https://github.com/callowayproject/bump-my-version>`_: As the name suggests, it bumps the version of your project following the `semver <https://semver.org/>`_ format and creates a new git tag.

Both of these tools' configurations are stored in the ``pyproject.toml`` file under the ``[tool.git-cliff]`` and ``[tool.bumpversion]`` sections, respectively.

Here is an example of the workflow:

Let's assume your project is at version ``0.0.1``, the initial version for new projects defined in the ``pyproject.toml`` file.
You make a few commits following the `conventional commits <https://www.conventionalcommits.org/en/v1.0.0/>`_ format, for example:

.. code-block:: shell
    :caption: Just an example to show commit messages

    git commit -m "feat: add new feature"
    git commit -m "fix: fix a bug"
    git commit -m "feat: add another feature"

Then you are ready for a minor release. Following the `semver <https://semver.org/>`_ convention, that is equivalent to moving from ``0.0.1`` to ``0.1.0``.
You run the following command:

.. code-block:: shell

    just bumpver minor

This will:

- bump the version of your project to ``0.1.0``
- update the ``CHANGELOG.md`` file with the latest commits
- create a new git tag with the name ``v0.1.0``
- push the tag to the remote repository, which will trigger the GitHub Action to create a new release with the content of the ``CHANGELOG.md`` file, build the binary and deploy the project.

.. note::

    A pre-commit hook keeps the ``CHANGELOG.md`` file updated with unreleased changes.