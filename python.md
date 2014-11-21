## Best Practices for Python

### Baseline

* [PEP8](http://www.python.org/dev/peps/pep-0008/).
* [Django Coding Style](https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/coding-style/).
* **Use single-quotes for strings.**

### Libraries

For consistency, prefer the following libraries to others that perform the same tasks:

* [Fabric](http://docs.fabfile.org/) for project tasks
* [Flask](http://flask.pocoo.org/) for light web apps **or** [Django](https://www.djangoproject.com/) for heavy web apps 
* [boto](https://github.com/boto/boto) for accessing AWS programmatically
* [pytz](http://pytz.sourceforge.net/) for manipulating timezones
* [psycopg2](http://www.initd.org/psycopg/) for accessing Postgres
* [lxml](http://lxml.de/) for XML/DOM manipulation
* [Requests](http://docs.python-requests.org/en/latest/) for working over HTTP

### Specifics

* When testing for nulls, always use ``if foo is None`` rather than ``if !foo`` so ``foo == 0`` does not cause bugs.
* Always initialize and store datetimes in the UTC timezone. Never use naive datetimes for any reason.
* Always use ``with`` when accessing resources that need to be closed.
* Always access blocking resources (files, databases) as little as possible.
* When accessing a dictionary element that may not exist, use ``get()``. For example, ``os.environ.get('DEPLOYMENT_TARGET', None)``.
* Project settings that will be used by both fabric and other code should be isolated in ``app_config.py``. ``fabfile.py`` and Django's ``settings.py`` should import from this file to prevent duplication.
* Imports should be organized into three blocks: stdlib modules, third-party modules and our own modules. Each group should be alphabetized.
* Avoid ``from foo import *``. It is the mindkiller.
* Functions that are de facto "private" (only called within a module or only called within a class) should be prefixed with a single `_`, e.g. `_slugify`.

### Flask

* All views should return with ``make_response``.
