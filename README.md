# NPR Apps' Best Practices and Coding Conventions



## Projects

### READMEs

* Document steps to setup the project from a blank slate.
* Document any required environment variables.
* Document any cron jobs that must be installed on the servers.



## HTML & CSS

* Element IDs and class names should always be ``lowercase-with-dashes``.
* Multi-part IDs and class names should always proceed from more general to more specific. For example, ``electris-skinny`` is better than ``skinny-electris``.
* Add CSS and Javascript to the asset compressor (e.g. ``assets_env.py``) instead of the HTML.
* Put all modals in the footer, followed by all javascript templates.



## Javascript

### General

* Use 4-spaces for indentation (because it's easier to be consistent with Python than switch your editor back and forth).
* Javascript variables names should always be ``lowercase_with_underscores``.
* Static variables and configuration parameters should be in ``TITLECASE_WITH_UNDERSCORES``.
* All global variables should be defined at the top of the file.
* All variables should be constrained to the current scope with ``var``.
* Declare only a single variable on one line.
* End all statements with a semicolon.
* Use spaces after opening and before closing braces and brackets in array and object definitions, i.e. ``{ foo: [ 1, 2, 3 ] }`` not ``{foo:[1,2,3]}``.
* When accessing properties of a data structure (such as one retrieved using ``getJSON``) prefer bracket syntax (``data["property"]``) to attribute syntax (``data.property``).
* Very frequent property references should be cached, i.e. ``var array_length = array.length;``.
* Use ``===`` rather than ``==``.
* **Use single-quotes for strings.**

### Libraries

For consistency, prefer the following libraries to others that perform the same tasks:

* [jQuery](http://jquery.com/)
* [Underscore](http://documentcloud.github.com/underscore/) (where Underscore and jQuery overlap, i.e. ``each()``, prefer Underscore)
* [Bootstrap](http://twitter.github.com/bootstrap/)
* [Moment](http://momentjs.com/)

### jQuery-specific

* jQuery references that are used more than once should be cached. Postfix these references with ``_el`` or ``_els``, i.e. ``var electris_el = $("#electris");``.
* Whenever possible constrain jQuery DOM lookups within the scope of a cached element. For example, ``electris_el.find(".candidate")`` is preferable to ``$(".candidate")``.
* Always use [on](http://api.jquery.com/on/), never [bind](http://api.jquery.com/bind/), [delegate](http://api.jquery.com/delegate/) or [live](http://api.jquery.com/live/). ``on`` should also be preferred to "verb events", such as [click](http://api.jquery.com/click/).

### Underscore-specific

* Always precompile your templates on page load, i.e. ``var STATE_TEMPLATE = _.template($("#state-template").html());``.



## Python

### Baseline

* [PEP8](http://www.python.org/dev/peps/pep-0008/).
* [Django Coding Style](https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/coding-style/).
* **Use single-quotes for strings.**

### Libraries

For consistency, prefer the following libraries to others that perform the same tasks:

* [Fabric](http://docs.fabfile.org/)
* [Django](https://www.djangoproject.com/) OR [Flask](http://flask.pocoo.org/)
* [boto](https://github.com/boto/boto)
* [pytz](http://pytz.sourceforge.net/)
* [psycopg2](http://www.initd.org/psycopg/)
* [lxml](http://lxml.de/)
* [Requests](http://docs.python-requests.org/en/latest/)

### Specifics

* When testing for nulls, always use ``if foo is None`` rather than ``if !foo`` so ``foo == 0`` does not cause bugs.
* Always initialize and store datetimes in the UTC timezone. Never use naive datetimes for any reason.
* Always use ``with`` when accessing resources that need to be closed.
* Always access blocking resources (files, databases) as little as possible.
* When accessing a dictionary element that may not exist, use ``get()``. For example, ``os.environ.get('DEPLOYMENT_TARGET', None)``.
* Project settings should be isolated in ``settings.py`` (whether by Django or manually). ``fabfile.py`` should import settings from this file to prevent duplication.
* Imports should be organized into three blocks: stdlib modules, third-party modules and our own modules. Each group should be alphabetized.



## git

* Development of major features should happen on separate branches which periodically merge *from* ``master`` until development of the feature is complete.
* A ``stable`` branch should always be present and should merge *from* ``master``, only when deploying to production.
* Don't store binary files (comps, databases) in the repository.
* If a binary object needs to be shared store it in Dropbox or on S3. If it is part of the setup process (e.g. a database backup) then use fabric commands to read and write it.
* **Never, ever store passwords, keys or credentials in any repository.** (Use environment variables instead.)


