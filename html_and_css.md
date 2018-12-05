## Best Practices for HTML and CSS


### HTML

* Element IDs and class names should always be `lowercase-with-dashes`.
* Put modals and JST templates in their own files. In the app-template these belong in the `templates` and `jst` directories, respectively. When this isn't feasible, put modals in the page footer first, followed by inline javascript templates.
* Attributes should be double-quoted, even in cases where it will parse without quotes.
* Images should always have an "alt" attribute that contains a screenreader-friendly description of their contents. If the image is only for presentational purposes, set the attribute to an empty string so that it will be ignored.
* Follow [WAI-ARIA best practices](https://www.w3.org/TR/wai-aria-practices/) for interactive elements, such as buttons, menus, or tabs. Test all HTML in a screenreader with this as a guide.
* Use semantic HTML for the appropriate purposes.
  - Never use a `<div>` for a clickable element. If the element updates the page, use a `<button>`, and if it navigates to another page, use `<a>`.
  - When possible, prefer built-in HTML elements (such as `<details>`, `<select>`, or `<input>`) to custom UI.
  - Use `<header>`, `<footer>`, `<main>`, `<section>`, and `<aside>` to mark up the document outline and provide navigation landmarks. Other HTML tags may be useful [depending on support](http://html5doctor.com/).
* Never set an element's "tabindex" to anything other than "-1" (for elements that are only focused via JavaScript) or "0" (for elements that should be made focusable as normal).

### LESS/CSS

* Prefer classes to IDs: the latter is an order of magnitude higher in specificity when ranking selectors, which makes it difficult to manage style conflicts.
* When managing stateful display styles, start from the ARIA best practices whenever possible. For example, when creating a toggle button, indicate its state based on `[aria-pressed="true"]` instead of adding an "active" class.
* Minimize nesting, but use it to create scoped styles for page components. Excessive nesting makes it difficult to override styles later on. A nested component should have all its base styles at the top, followed by descendant styles. Do not intersperse descendent styles and top-level styles.
* Mobile styles should be nested inside the component they override, as close to the overridden style as possible.
* Use variables whenever possible for colors, media queries, shadows, and other shared visual traits. Consider using [CSS custom properties](https://developer.mozilla.org/en-US/docs/Web/CSS/--*) for any styles that change based on state in multiple places.
* Never override the `:focus` selector. Always provide styles for both `:hover` and `:focus` on any interactive element.