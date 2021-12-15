Testing Basics
================

``pytest-django`` Quick Start
------------------------------

pytest-django is a plugin for `pytest <https://pytest.org/>`_ that provides a 
set of useful tools for testing Django applications and projects. For more information
see the `pytest-django documentation <https://pytest-django.readthedocs.io/en/latest/>`_.

To install pytest-django:

.. code-block:: console

    $ pip install -U pytest-django
    ...

The ``DJANGO_SETTINGS_MODULE`` environment variable must be defined and should point
to the django settings module used for testing. There are different approaches towards
configuration.

1. Use ``pytest.ini`` to define ``DJANGO_SETTINGS_MODULE`` variable:

    .. code-block:: ini

        # -- FILE: pytest.ini (or tox.ini)
        [pytest]
        DJANGO_SETTINGS_MODULE = myapp.tests.settings
        # -- recommended but optional:
        python_files = tests.py test_*.py *_tests.py

2. Set ``DJANGO_SETTINGS_MODULE`` environment variable, e.g. in bash or Windows Command prompt

    .. code-block:: console

        $ export DJANGO_SETTINGS_MODULE = myapp.tests.settings
        $ pytest

3. Use ``pytest_configure`` hook 

    .. code-block:: python

        # -- FILE: conftest.py
        import os
        from pathlib import Path
        import sys
        import django

        TESTS_DIR = Path(__file__).resolve().parent
        BASE_DIR = TESTS_DIR.parent
        sys.path.insert(0, BASE_DIR.absolute())

        def pytest_configure():
            os.environ['DJANGO_SETTINGS_MODULE'] = 'myapp.tests.settings'
            django.setup()

    See also `pytest_configure hook <https://docs.pytest.org/en/6.2.x/reference.html#pytest.hookspec.pytest_configure>`_

4. Use ``--ds=SETTING`` command line option

    .. code-block:: console

        $ pytest --ds=myapp.tests.settings

    Variant to this method is to add the command line option to ``pytest.ini``:

    .. code-block:: ini

        addopts = --ds=myapp.tests.settings

How the setting is choosen? The precedence (from higher to lower) is as follows:

- The command line option ``--ds``
- The command line option ``--ds`` from ``pytest.ini`` or ``tox.ini``
- The environment variable ``DJANGO_SETTINGS_MODULE``
- The ``DJANGO_SETTINGS_MODULE`` option in the configuration file - ``pytest.ini`` (``tox.ini``)

To override settings you can use the ``setting`` fixutre.

.. code-block:: python

    @pytest.fixture(autouse=True)
    def use_dummy_cache_backend(settings):
        settings.CACHES = {
            "default": {
                "BACKEND": "django.core.cache.backends.dummy.DummyCache",
            }
        }


``settings`` module for testing
-------------------------------

.. code-block:: python

    import os

    # Define environment variables for testing
    os.environ['DJANGO_SECRET_KEY'] = 'secret-key'

    # Get everything from production settings module
    from myproj.settings import *

    # Use in-memory database for better testing performance
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': ':memory:',
        }
    }


Testing with database
----------------------

In order for a test to have access to the database it must either be marked using the django_db() 
mark or request one of the db, transactional_db or django_db_reset_sequences fixtures. Otherwise 
the test will fail when trying to access the database.

``pytest.mark.django_db`` is used to mark a test function as requiring the database. 
It will ensure the database is set up correctly for the test. Each test will run in its own 
transaction which will be rolled back at the end of the test. This behavior is the same as 
Django's standard TestCase class.

.. code-block:: python

    @pytest.mark.django_db
    def test_user_model():
        user = User(username='joe')
        assert str(user) == user

Testing with client
--------------------

Use ``client`` fixture - An instance of a `django.test.Client <https://docs.djangoproject.com/en/stable/topics/testing/tools/#django.test.Client>`_.

.. code-block:: python

    def test_with_client(client):
        response = client.get('/')
        assert response.status_code == 200

For situations where logged in user is required, following are useful:

- ``admin_user`` fixture - An instance of a superuser, with username “admin” and password “password” (in case there is no “admin” user yet).
- ``admin_client`` fixture - An instance of a `django.test.Client <https://docs.djangoproject.com/en/stable/topics/testing/tools/#django.test.Client>`_, logged in as an admin user.
- ``client.force_log()`` method

.. code-block:: python

    def test_authenticated(client, admin_user):
        client.force_login(admin_user)
        response = client.get('/admin/')
        assert response.status_code == 200

    def test_authenticated(admin_client):
        response = client.get('/admin/')
        assert response.status_code == 200


Further information:

- `Client class <https://docs.djangoproject.com/en/4.0/topics/testing/tools/#django.test.Client>`_
- `Response class <https://docs.djangoproject.com/en/4.0/topics/testing/tools/#django.test.Response>`_
