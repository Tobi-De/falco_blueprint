Authentication
==============

Login Required
---------------

The `LoginRequiredMiddleware <https://docs.djangoproject.com/en/5.1/ref/middleware/#module-django.contrib.auth.middleware>`_ is enabled to make all views require login
by default. To make a view public, you use the `@login_not_required <https://docs.djangoproject.com/en/5.1/topics/auth/default/#django.contrib.auth.decorators.login_not_required>`_ decorator.

.. code-block:: python
    :caption: Example

    from django.contrib.auth.decorators import login_not_required

    @login_not_required
    def public_view(request):
        return HttpResponse("This is a public view")


Login via email instead of username
-----------------------------------

The ``email`` field is configured as the login field using `django-allauth <https://github.com/pennersr/django-allauth>`_. The ``username`` field is still present
but is not required for login. Allauth automatically fills it with the part of the email before the ``@`` symbol.
More often then not when I create a new django project I need to use something other than the ``username`` field provided by django as the unique identifier of the user,
and the ``username`` field just becomes an annoyance to deal with. It is also more common nowadays for modern web and mobile applications to rely on a unique identifier
such as an email address or phone number instead of a username.

.. important::

    There is a small fix applied for allauth related to django-fastdev in the ``core/apps.py`` file. Make sure to read it in case you ever need to change it.

.. admonition:: Custom user model
    :class: note dropdown

     I also removed the ``first_name`` and ``last_name`` fields that are available by default on the Django ``User`` model. I don't always need them, and when I do, I generally have a separate ``Profile``
     model to store users' personal informations, keeping the ``User`` model focused on authentication and authorization.
     My reasoning for this is to avoid asking for unnecessary data (following the principle of `YAGNI <https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it>`_). A positive consequence of this approach
     is that having less data on your users/customers increases the chances of being `GDPR compliant <https://gdpr.eu/compliance/>`_. You can always add these fields later if needed.

     -- me, not so long ago

    Previously, this section of the docs contained the message above. Now, I take a simpler approach: Falco doesn't ship with a custom user model anymore, and I don't recommend having one for most people. There are
    now even better resources I can link to that explain why this is better than I could ever do:

    - https://noumenal.es/posts/django-unique-user-email/928/
    - https://buttondown.com/carlton/archive/evolving-djangos-authuser/

    If you need to save user data, a profile model is a better approach, and better field names are ``full_name`` and ``short_name``. For the reasoning behind this, check out
    https://django-improved-user.readthedocs.io/en/latest/rationale.html