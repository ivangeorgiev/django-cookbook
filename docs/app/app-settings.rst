App Settings
==============

Django provides project ``settings`` module for configuration. When application needs configuration settings it is not good idea to directly refer to the project's ``settings`` module as this creates unnecessary dependencies. Because this dependency is spread across all the code things are even worse. Instead, create ``settings`` or ``config`` configuration module local to the application and refer to it:

.. code-block:: python

   from django.conf import settings
   from django.contrib import auth

   SITE_TITLE = "My Prescious!"
   SYSTEM_USERNAME = "SYSTEM"
   USER_MODEL = auth.get_user_model()

   globals().update(getattr(settings, "MY_PRESCIOUS", {}))

The local configuration module provides meaningful defaults for application settings and also enables settings to be overriden by project settings (project settings take precedence).

In project ``settings`` module you can override application settings, using dictionary:

.. code-block:: python

   #...
   MY_PRESCIOUS = {
      "SITE_TITLE": "Bilbo's Adventure",
   }
   #...

When you need to use configuration settings, refer the application's local configuration modle:

.. code-block:: python

   from . import settings

   system_user = settings.USER_MODEL.objects.get(username=USER_MODEL.SYSTEM_USERNAME)

This approach reduces the coupling between the application and the project. It also isolates the application dependency on the project settings in one place.
