CI / CD
=======

Continuous Integration
----------------------

The file at ``.github/workflows/ci.yml`` is responsible for `Continuous Integration <https://en.wikipedia.org/wiki/Continuous_integration>`_.
Every time you push new changes to the main branch or create pull requests, an action is triggered to run tests, deployment checks, and type checks. This ensures nothing has broken
from the previous commit (assuming you write tests).
The content of the file is quite simple to read and understand. The main thing to note is that the workflow file only contains Just recipe commands. The actual commands are all defined in the justfile, so that you can easily run them locally if needed
or migrate to another CI/CD provider if you want to.

.. code-block:: shell
    :caption: Example of commands related to CI

    just types # run type checks with mypy
    just test # run tests with pytest
    just deploy-checks # run django deployment checks
    just runci # run all the above commands in sequence


Continuous Deployment
---------------------

There is a ``.github/workflows/cd.yml`` file that defines GitHub Actions that run every time you push new tags to your repository. This will push your changes to the server,
build wheels and binary for the project, and create a new GitHub release with the latest content from the ``CHANGELOG.md`` file.