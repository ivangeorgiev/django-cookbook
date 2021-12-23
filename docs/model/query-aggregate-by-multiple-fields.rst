Aggregate by multiple fields
=============================

We have a model which describes incidents.

.. code-block:: python

   class Incident(Model):
      id = models.BigAutoField(primary_key=True)
      title = models.CharField(max_length=1024, blank=True, null=True)
      description = models.TextField(blank=True, null=True)
      severity = models.CharField(max_length=32, choices=shortcuts.get_enum_choices(IncidentSeverity))
      status = models.CharField(max_length=32, choices=shortcuts.get_enum_choices(IncidentStatus))

We need to get the number of incidents for each distinct ``status``, ``severity`` pair. Expressed in SQL this would look like:

.. code-block:: sql

   SELECT status, severity, count(*) AS num_incidents
    GROUP BY status, severity

Solution is to use the `values() method`_ to first group the objects from the query set and than use ``annotate()`` to aggreate.

.. code-block:: pycon

      >>> summary = Incident.values("status", "severity").annotate(num_incidents=Count("*"))
      >>> summary
      {'severity': 'critical', 'status': 'new', 'num_incidents': 1}

See `values() method`_.

.. _values() method: https://docs.djangoproject.com/en/4.0/topics/db/aggregation/#values
