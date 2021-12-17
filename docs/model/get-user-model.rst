Get User Model
===============

Default user model in Django is `django.contrib.auth.models.User`_. Django allows you to override the default user model by providing a value for the ``AUTH_USER_MODEL`` setting that references a custom model:

.. code-block:: python

   AUTH_USER_MODEL = 'myapp.MyUser'

When the application needs to refer to the user model, do not directly refer to `User`_, but use the `get_user_model()`_ function.

.. code-block:: python

   from django.contrib.auth import get_user_model

   system_user = get_user_model().objects.get(username='SYSTEM')


.. _django.contrib.auth.models.User: User_
.. _get_user_model(): https://docs.djangoproject.com/en/4.0/topics/auth/customizing/#django.contrib.auth.get_user_model
.. _User: https://docs.djangoproject.com/en/4.0/ref/contrib/auth/#django.contrib.auth.models.User

