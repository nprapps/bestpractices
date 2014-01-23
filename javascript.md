## Best Practices for Javascript

### General

* Use 4-spaces for indentation (because it's easier to be consistent with Python than it is to switch your editor back and forth).
* Javascript variables names should always be ``camelCase`` (not ``snake_case``).
* Static variables and configuration parameters should be in ``TITLECASE_WITH_UNDERSCORES``.
* Named functions should look like this ``var functionName = function() {}``.
* All global variables should be defined at the top of the file.
* All variables should be constrained to the current scope with ``var``.
* Declare only a single variable on one line.
* End all statements with a semicolon.
* Use spaces after opening and before closing braces and brackets in array and object definitions, i.e. ``{ foo: [ 1, 2, 3 ] }`` not ``{foo:[1,2,3]}``.
* Do not use spaces after opening or before closing parentheses, i.e. ``if (foo === true) {`` and not ``if ( foo === true ) {``. 
* When accessing properties of a data structure (such as one retrieved using ``getJSON``) prefer bracket syntax (``data['property']``) to attribute syntax (``data.property``).
* Very frequent property references should be cached, i.e. ``var array_length = array.length;``.
* Use ``===`` rather than ``==``. ([Why?](http://www.impressivewebs.com/why-use-triple-equals-javascipt/))
* **Use single-quotes for strings.**

### Libraries

For consistency, prefer the following libraries to others that perform the same tasks:

* [jQuery](http://jquery.com/) for DOM manipulation
* [Underscore.js](http://documentcloud.github.com/underscore/) for functional programming (where Underscore and jQuery overlap, i.e. ``each()``, prefer Underscore)
* [Bootstrap](http://twitter.github.com/bootstrap/) for responsiveness
* [Moment.js](http://momentjs.com/) for datetime handling
* [jPlayer](http://jplayer.org/) for audio/video playback

### jQuery-specific

* jQuery references that are used more than once should be cached. Prefix these references with ``$``, i.e. ``var $electris = $('#electris');``.
* Whenever possible constrain jQuery DOM lookups within the scope of a cached element. For example, ``$electris.find('.candidate')`` is preferable to ``$('.candidate')``.
* Always use [on](http://api.jquery.com/on/), never [bind](http://api.jquery.com/bind/), [delegate](http://api.jquery.com/delegate/) or [live](http://api.jquery.com/live/). ``on`` should also be preferred to "verb events", such as [click](http://api.jquery.com/click/).

