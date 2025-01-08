HTMX and Template partials
==========================

The project comes set up with django-template-partials_ and htmx_ for the times when you need to add some
interactivity to your web app. The `interactive user interfaces guide <https://falco.oluwatobi.dev/guides/interactive_user_interfaces.html>`_ goes into more detail on this, but for a brief overview:

* django-template-partials_ is used to define reusable fragments of HTML
* htmx_'s job is to make requests to the backend, get a piece of HTML fragment in response, and patch the `DOM <https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction>`_ using it. Basically, htmx allows you to write declarative code to make `AJAX <https://www.w3schools.com/xml/ajax_intro.asp>`_ (Asynchronous JavaScript And XML) requests.

.. admonition:: jetbrains extensions
    :class: tip dropdown

    If you are using a jetbrains IDE, there is an extension that add support for htmx, you can find it `here <https://plugins.jetbrains.com/plugin/20588-htmx-support>`_.
    There is also `this extension <https://plugins.jetbrains.com/plugin/15251-alpine-js-support>`_ for `alpinejs <https://alpinejs.dev/>`_  support.

Let's look at a quick example:

.. code-block:: django
   :linenos:
   :caption: elements.html
   :emphasize-lines: 4, 6, 11-13


   {% block main %}
   <ul id="element-list">
      {% for el in elements %}
         {% partialdef element-partial inline=True %}
            <li>{{ el }}</li>
         {% endpartialdef %}
      {% endfor %}
   </ul>

   <form
   hx-post="{% url 'add_element' %}"
   hx-target="#element-list"
   hx-swap="beforeend"
   >
      <!-- Let's assume some form fields are defined here -->
      <button type="submit">Submit</button>
   </form>

   {% endblock main %}

The htmx attributes (prefixed with ``hx-``) defined above basically say:

 when the form is submitted, make an asynchronous JavaScript request to the URL ``{% url 'add_element' %}`` and add the content of the response before the end (before the last child) element of the element with the ID ``element-list`` .

The complementary Django code on the backend would look something like this:

.. code-block:: python
   :linenos:
   :caption: views.py
   :emphasize-lines: 6

   def add_element(request):
      new_element = add_new_element(request.POST)
      if request.htmx:
         return render(request, "myapp/elements.html#element-partial", {"el": new_element})
      else:
         redirect("elements_list")

The highlighted line showcases a syntax feature provided by django-template-partials_. It enables you to selectively
choose the specific HTML fragment from the ``elements.html`` file that is enclosed within the ``partialdef`` tag with the name ``element-partial``.

The ``htmx`` attribute on the ``request`` element is provided by django-htmx_, which is already configured in the project.

This example illustrates how you can create a button that adds a new element to a list of elements on a page without reloading the entire page.
Although this might not seem particularly exciting, the `interactive user interfaces guide <https://falco.oluwatobi.dev/guides/interactive_user_interfaces.html>`_ provides more
practical examples that demonstrate the extensive possibilities offered by this approach.


.. _django-template-partials: https://github.com/carltongibson/django-template-partials
.. _htmx: https://htmx.org/
.. _django-htmx: https://github.com/adamchainz/django-htmx