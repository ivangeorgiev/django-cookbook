Application Name in Admin
=================================

By default Django uses the application label as application's verbose name. It is also possible to customize the application's verbose name by assigning a value to the ``verbose_name`` of the applications config (AppConfig_ class).

To achieve this, modify the application's ``admin`` module and assign value to the ``verbose_name`` attribute of the AppConfig_ class:

.. code-block:: python

   from django.apps import AppConfig

   class MypresciousConfig(AppConfig):
      #...
      verbose_name = "Bilbo's Adventures"
      #...

.. _AppConfig: https://docs.djangoproject.com/en/4.0/ref/applications/#django.apps.AppConfig
