Models
======

Model lifecycle
---------------

`django-lifecycle <https://github.com/rsinger86/django-lifecycle>`_ offers an alternative to `signals <https://docs.djangoproject.com/en/dev/topics/signals/>`_ for hooking into your model's lifecycle.
It provides a more readable and understandable way to write code that runs before or after a model instance is created or updated, based on certain conditions. This code is placed directly on
the concerned models, which aligns well with Django's `fat models` philosophy.

Here is an example of using ``django-lifecycle`` straight from their README:

.. code-block:: python

   from django_lifecycle import LifecycleModel, hook, BEFORE_UPDATE, AFTER_UPDATE
   from django_lifecycle.conditions import WhenFieldValueIs, WhenFieldValueWas, WhenFieldHasChanged


   class Article(LifecycleModel):
      contents = models.TextField()
      updated_at = models.DateTimeField(null=True)
      status = models.ChoiceField(choices=['draft', 'published'])
      editor = models.ForeignKey(AuthUser)

      @hook(BEFORE_UPDATE, WhenFieldHasChanged("contents", has_changed=True))
      def on_content_change(self):
         self.updated_at = timezone.now()

      @hook(AFTER_UPDATE,
        condition=(
            WhenFieldValueWas("status", value="draft")
            & WhenFieldValueIs("status", value="published")
        )
      )
      def on_publish(self):
         send_email(self.editor.email, "An article has published!")

