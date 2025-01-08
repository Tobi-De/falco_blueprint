Background Tasks
================

.. danger::
    outdated, django-tasks is use now

`django-q2 <https://github.com/django-q2/django-q2>`_ is my preferred background task queue system for Django. In most projects, I always utilize either the task queue processing,
scheduling, or sometimes both. Regarding scheduling, there is also `django-q-registry <https://github.com/westerveltco/django-q-registry>`_ included, which is a ``django-q2`` extension
that helps with easily registering scheduling jobs.

Here is an example of how using both looks:

.. tabs::

    .. tab:: tasks.py

        .. code-block:: python
            :caption: tasks.py

            from django.core.mail import send_mail
            from django_q.models import Schedule
            from django_q_registry import register_task

            @register_task(
                name="Send periodic test email",
                schedule_type=Schedule.MONTHLY,
            )
            def send_test_email():
                send_mail(
                    subject="Test email",
                    message="This is a test email.",
                    from_email="noreply@example.com",
                    recipient_list=["johndoe@example.com"],
                )


            def long_running_task(user_id):
                # a simple task meant to be run in background
                ...

    .. tab:: views.py

        .. code-block:: python
            :caption: views.py

            from django_q.tasks import async_task
            from .tasks import long_running_task

            def my_view(request):
                task_id = async_task(long_running_task, user_id=request.user.id)
                ...

It is a good idea to organize any task or scheduling job function in a ``tasks.py`` file in the relevant Django application.

.. hint::

    For more details on task queues and scheduling, check out `my guide on the topic <https://falco.oluwatobi.dev/guides/task_queues_and_schedulers.html/>`_.
