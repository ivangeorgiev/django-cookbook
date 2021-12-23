Rename serialized field from model
====================================

We are given following model:

.. code-block:: python

   from rest_framework import serializers

   class System(BaseModel):
      """Define systems reflected on the status page."""

      system_id = models.BigAutoField(primary_key=True, verbose_name="id")
      name = models.CharField(max_length=255)
      description = models.TextField(null=True, blank=True)

   class SystemSerializer(serializers.ModelSerializer):
      """Serializer for the System model."""

      class Meta:
         """Meta options for the serializer."""
         model = models.System
         fields = "__all__"

We want that in the serialized output field ``system_id`` is renamed to ``id``.

To achieve this, use the ``source`` parameter of the ``Field`` class:

.. code-block:: python

   from rest_framework import serializers

   class SystemSerializer(serializers.ModelSerializer):
      """Serializer for the System model."""

      id = serializers.IntegerField(source="system_id")

      class Meta:
         """Meta options for the serializer."""
         model = models.System
         fields = ("id", "name", "description")
