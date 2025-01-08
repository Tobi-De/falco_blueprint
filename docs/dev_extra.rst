Development Extra
=================

DjangoFastDev
-------------

The DjangoFastDev package helps catch small mistakes early in your project. When installed you may
occasionally encounter a ``FastDevVariableDoesNotExist`` error, this exception is thrown during template rendering
by `django-fastdev <https://github.com/boxed/django-fastdev>`_ when you try to access a variable that is not defined in the context
of the view associated with that template. This is intended to help you avoid typos and small errors that will
have you scratching your head for hours, read the project `readme <https://github.com/boxed/django-fastdev#django-fastdev>`_ to see
all the features it provides.
If you find the package's errors to be too frequent or annoying, you can disable it by removing the ``django-fastdev`` application
entirely or by commenting it out in the ``settings.py`` file.


.. code:: python

   THIRD_PARTY_APPS = [
       ...
       # 'django_fastdev',
   ]


Dj Notebook
-----------

This package allows you to use your `shell_plus <https://django-extensions.readthedocs.io/en/latest/shell_plus.html>`_ in a Jupyter notebook.
In the root of the generated project, you will find a file named ``playground.ipynb`` which is configured with dj-notebook_.
As the name suggests, I use this as a playground to play with the Django ORM. Having it saved in a file is particularly useful for storing frequently used queries in text format,
eliminating the need to retype them or search through command line history. Before running any additional cells you add, make sure to run the first cell in the notebook to set up Django. It's
important to note that dj-notebook_ does not automatically detect file changes, so you will need to restart the kernel after making any code modifications.
If you need a refresher on Jupyter notebooks, you can refer to this `primer <https://www.dataquest.io/blog/jupyter-notebook-tutorial/>`_.

**Marimo**

There is a new alternative to Jupyter notebooks, namely, `marimo <https://marimo.io/>`_. The main features that I appreciate are:

- Notebooks are straightforward Python scripts.
- It has a beautiful UI.
- It provides a really nice tutorial: ``pip install marimo && marimo tutorial intro``.

Its main advertised feature is having reactive notebooks, but for my use case in my Django project, I don't really care about that.

If you want to test ``marimo`` with your Django project, it's quite simple. Install it in your project environment and run:

.. code-block:: shell

   marimo edit notebook.py

Or using uv:

.. code-block:: shell

   uvx marimo edit notebook.py

As with ``dj-notebook``, for your Django code to work, you need some kind of activation mechanism. With ``dj-notebook``, the first cell needs to run the code ``from dj_notebook import activate; plus = activate()``. With ``marimo``, the cell below should do the trick.

.. code-block:: python

   import django
   import os

   os.environ["DJANGO_SETTINGS_MODULE"] = "<your_project>.settings"
   django.setup()



.. _dj-notebook: https://github.com/pydanny/dj-notebook