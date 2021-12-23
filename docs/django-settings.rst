Django Settings
================

ALLOWED_HOSTS Wildcards
-------------------------

``ALLOWED_HOSTS`` is a list of strings representing host names that this Django site can serve. Entires in ``ALLOWED_HOSTS`` list could be:

- fully qualified domain names, including IP addresses if the site is being accessed by IP address;
- a value beginning with a period will match the given domain and any subdomain of the given domain. E.g. ``.igeorgiev.eu`` will match any subdomain, including ``www.igeorgiev.eu``, ``django.igeorgiev.eu``, ``igeorgiev.eu``.
- a value of ``*`` will match anything. No hosts header validation is done.


For more information see `Djanto settings documentation <https://docs.djangoproject.com/en/2.2/ref/settings/#allowed-hosts>`__.
