Readonly Model Admin
=====================

In the ``admin`` module override ``has-*-permission`` methods for the model admin to return ``False``.

.. code-block:: python

   class StatusPageAdmin(admin.ModelAdmin):
      # ...

      def has_add_permission(self, *args, **kwargs) -> bool:
          """Disable add new object from the UI."""
          return False

      def has_change_permission(self, *args, **kwargs) -> bool:
          """Disable object update from the UI."""
          return False

      def has_delete_permission(self, *args, **kwargs) -> bool:
          """Disable object delete from the UI."""
          return False

For more information refer to the ``ModelAdmin`` `documentation <https://docs.djangoproject.com/en/4.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.has_view_permission>`__.

