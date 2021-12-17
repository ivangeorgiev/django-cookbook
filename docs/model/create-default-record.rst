Create Default Object
======================

My application should use system user. How I can make sure that such user exists?

Django sends `post_migrate`_ signal. You need to register a receiver to that signal.

Step 1. Define signal receiver
------------------------------

It is a good idea to collect all signal processing logic of your application into a single module. We are using :ref:`signals-py`.

.. code-block:: python
   :caption: ``signals`` module
   :name: signals-py
   :emphasize-lines: 14

   """Register receivers for Django signals."""

   from django.db.models.signals import post_migrate

   from . import services


   def post_migrate_receiver(**kwargs):
       """Receive post_migrate signal."""
       services.ensure_system_user()

   def register_receivers():
       """Register signal recievers."""
       post_migrate.connect(post_migrate_receiver)

Usually signal receivers are registered during ``signals`` module load, but this is not a good practice. Better is to create a function which is responsible for receiver registration.

Implementation of :ref:`services-py` might look like following:

.. code-block:: python
   :name: services-py
   :caption: ``services`` module

   from django.contrib import auth
   from django.core.exceptions import ObjectDoesnotExist

   SYSTEM_USERNAME = "SYSTEM"

   def get_system_user():
      """Get system user object."""
      user_model = auth.get_user_model()
      return user_model.objects.get(username=SYSTEM_USERNAME)

   def ensure_system_user():
      """Make sure system user exists."""
      try:
         get_system_user()
      except ObjectDoesNotExist:
         user_model = auth.get_user_model()
         system_user = user_model.objects.create(username=SYSTEM_USERNAME)


Step 2. Register signal listeners
----------------------------------

You can register signals once the application is ready. For this override the ``ready()`` method of your ``AppConfig`` in :ref:`apps-py` module.

.. code-block:: python
   :caption: ``apps`` module
   :name: apps-py
   :emphasize-lines: 4,6

   # ...
   class DjangostatuspageConfig(AppConfig):
      #...
      def ready(self):
         """Perform application initialization post Django ready."""
         signals.register_receivers()


.. _post_migrate: https://docs.djangoproject.com/en/3.0/ref/signals/#post-migrate

