Set Plural Verbose Name for Model
==================================

When Django needs to display the model name in plural, by default it adds "s" to the verbose name of the model.

In some situations you might need to override this behavior. For example "Category" model should be "Categories" and not "Categorys".

To set plural verbose name for a model, set the `verbose_name_plural` attribute for the `model's meta <https://docs.djangoproject.com/en/4.0/ref/models/options/#verbose-name-plural>`__:

.. code-block:: python

   class ArticleCategory(models.model):
       # ...
       class Meta:
           verbose_name_plural = "article categories"

For more details on the model's meta options, see the `documentation <https://docs.djangoproject.com/en/4.0/ref/models/options/>`__.
