First model or QuerySet Object
======================================

To get the first object from QuerySet_, use the `first <https://docs.djangoproject.com/en/4.0/ref/models/querysets/#first>`__ method:

.. code-block:: python

   p = Article.objects.order_by('title', 'pub_date').first()

In case the QuerySet_ contains only a single object, you could use directly:

.. code-block:: python

   p = Article.first()

.. _QuerySet: https://docs.djangoproject.com/en/4.0/ref/models/querysets/
