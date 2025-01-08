Documentation
=============

The documentation uses a basic `sphinx <https://www.sphinx-doc.org/en/master/>`_ setup with the `furo <https://github.com/pradyunsg/furo>`_ theme.
There is a basic structure in place that encourages you to structure your documentation based on your `django applications <https://docs.djangoproject.com/en/dev/ref/applications/>`_.

The `myst-parser <https://myst-parser.readthedocs.io/en/latest/>`_ is configured so you can write using either `reStructuredText <https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html>`_ or `markdown <https://www.markdownguide.org/>`_.
Even if you are not planning to have very detailed and highly structured documentation (for some ideas on that, check out the `documentation writing guide </guides/writing_documentation.html>`_),
it can be a good place to keep notes on your project architecture, setup, external services, etc. It doesn't have to be optimal to be useful.

 "The Palest Ink Is Better Than the Best Memory."

 --- Chinese proverb

.. code-block:: shell
    :caption: Example of commands related to documentation

    just docs-build # build the documentation into a static site
    just docs-serve # serve the documentation locally on port 8001
    just docs-upgrade # upgrade the documentation dependencies