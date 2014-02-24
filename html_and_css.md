## Best Practices for HTML and CSS

* Element IDs and class names should always be ``lowercase-with-dashes``.
* Put modals and JST templates in their own files. In the app-template these belong in the `templates` and `jst` directories, respectively. When this isn't feasible, put modals in the page footer first, followed by inline javascript templates.

### Bootstrap grid

* Use Bootstrap's [LESS mixins](http://getbootstrap.com/css/#grid-less) for constructing a grid. Never use row and column classes in your HTML so that we can keep things semantic. For example, a basic text block would look like this:

`HTML`:

```
<div class="text-wrapper">
    <div class="text">
        Text here.
    </div>
</div>
```

`LESS`:

```
.text-wrapper {
    .make-row();
    .text {
        .make-md-column(8);
        .make-md-column-offset(2);
    }
}