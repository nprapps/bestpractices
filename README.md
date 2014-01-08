# NPR Visuals' Best Practices

The contents of this repository are released under a [Creative Commons CC BY 3.0 License](http://creativecommons.org/licenses/by/3.0/deed.en_US).

**Note:** These best practices reference the [app-template](http://github.com/nprapps/app-template) in several places and generally assume you are using it.

## Projects

### READMEs

* Document steps to setup the project from a blank slate. (Data loading, etc.) This should include paths to files stored in Dropbox when relevant.
* Document any required environment variables. If these are secrets they should also be stored in the team Dropbox.
* Document any cron jobs that must be installed on the servers. In the app-template this just means using the `crontab` file in the project root.
* Document any server dependencies that are not part of our standard stack. This includes documenting how to install them. Whenever feasible this documentation should be in the form of `fab` commands.



## Naming things

Naming things (variables, files, classes, etc.) consistently and intuitively is one of the hardest problems in computer science. To make it easier, follow these conventions:

* Always proceed from more general to more specific. For example, ``widget-skinny`` is better than ``skinny-widget``.
* Strive for parallelism. If you have a `begin()` function, then have an `end()` function (not `stop()` or `done()`).
* Group related names with common prefixes, e.g. `search_query` and `search_address`.
* Prefer more specific terms to more vague ones. If it's an address call it `address`, not `location`.
* When a function operates on a variable, their naming should be consistent. If working with `updates` then `process_updates()`, don't `process_changes()`. 
* Maintain naming conventions between lists and their iterators: `for update in updates`, not `for record in updates`.

<table>
  <tr><th>Prefer...</th><th>to...</th></tr>
  <tr><td>create</td><td>insert, add, new</td></tr>
  <tr><td>update</td><td>change, edit</td></tr>
  <tr><td>delete</td><td>remove, purge</td></tr>
  <tr><td>setup</td><td>init</td></tr>
  <tr><td>make</td><td>build, generate</td></tr>
  <tr><td>wrapper</td><td>wrap</td></tr>
  <tr><td>render</td><td>draw</td></tr>
</table>  

(Note: sometimes these words don't mean the same thing, but when they do, prefer the former.)



## HTML & CSS

* Element IDs and class names should always be ``lowercase-with-dashes``.
* Put modals and JST templates in their own files. In the app-template these belong in the `templates` and `jst` directories, respectively. When this isn't feasible, put modals in the page footer first, followed by inline javascript templates.



## Javascript

See [javascript.md](https://github.com/nprapps/bestpractices/blob/master/javascript.md)


## Python

See [python.md](https://github.com/nprapps/bestpractices/blob/master/python.md)


## git

* Development of major features should happen on separate branches which periodically merge *from* ``master`` until development of the feature is complete.
* A ``stable`` branch should always be present and should merge *from* ``master``, only when deploying to production.
* Don't store binary files (comps, databases) in the repository.
* If a binary object needs to be shared then store it in Dropbox or on S3. If it is part of the setup process (e.g. a database backup) then use fabric commands to read and write it.
* **Never, ever store passwords, keys or credentials in any repository.** (Use environment variables instead.)



## Servers

* Environment variables belong in `/etc/environment`. This file should be sourced by cron jobs. (This happens automatically when using `run_on_server.sh`.)
