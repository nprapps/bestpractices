# Best Practices for Javascript

## General

Broadly speaking, the [Google JS style guide](https://google.github.io/styleguide/jsguide.html) is a reasonable starting place, but it's not dogma.

*Above all, our priority is clarity of intent.* That means naming variables with descriptive, clear names; splitting code into short, reusable modules; and keeping the flow of data clear. By contrast, the kinds of quotes you use, how you put spaces around keywords or objects, or whether you like to declare functions using expressions or variable assignment--none of these things significantly alter the clarity of the code. If there's a style issue that is particularly contentious, we will go with whatever [Prettier](https://prettier.io) emits, and you may want to install its plugin for your editor of choice.

The TL;DR of our style:

* Indentation: two spaces, no tabs.
* `camelCase`, not `snake_case` for variable names, `SHOUTING_SNAKE_CASE` for constants.
* End all statements with a semicolon. Do not use comma-first style in object or array literals.
* Avoid global variables and state whenever possible.
* We support whatever browsers are officially supported for the main site. Currently, this is all modern browsers (IE not included).
* Prefer clear (even overly-verbose) code over "clever" one-liners and obfuscated syntax.

On the topic of clarity, one place where it is important to be clear is when doing typecasting to and from primitive values. For these, rather than the shorthand operators, it's better to spell out the conversion explicitly. 

```js
// not this:
var n = +n;
var s = s + "";

// this:
var n = Number(n);
var s = String(s);

// use map for bulk conversions
var numericalValues = stringValues.map(Number);
```

The one place that we may use the shorthand is when converting to an integer, since there is no explicit integer type in JavaScript. In this case, you may use `var int = n | 0;` to do the conversion (or `Math.floor()`).

## ES2015+ syntax

We prefer to use new syntax where it will add expressiveness, and where it does not incur a size or speed penalty due to transpilation. The [Babel ES2015 guide](https://babeljs.io/docs/en/learn) is helpful for learning these new features, as well as determining when they will create overly-complicated output code.

Currently we avoid async/await and generators until support for them is more widespread.

### `let` and `const`

Use these if you're comfortable with them. `var` remains acceptable, if you understand the quirks around its scoping. All variable should be declared with one of the three--never use the global assignment form, and try not to use global state if possible.

### Arrow functions

Arrow functions should primarily be used in two cases: when we want to preserve the value of `this`, or when the function is a single expression. The latter case is particularly useful in D3, or for `map` and `reduce`. If the body expands to multiple lines, or if you need to return an object, use a regular function expression.

```js
// use arrows for short, pure functions
d3Container
  .select("rect")
  .data(data)
  .enter()
    .append("circle")
    .attr("fill", d => colorScale(d.label))
    .attr("x", d => xScale(d.x))
    .attr("y", d => yScale(d.y));

// Also useful for loops
elements.forEach(el => el.style.background = "red");

// Returning objects with arrows is hard to read:
keys.map(key => ({
  key,
  value: data[key]
}));

// use a full function instead
keys.map(function(key) {
  var value = data[key];
  return { key, value };
});
```

### Destructuring, spread, and object literals

When possible, use destructuring to pull out properties from an object or array, especially when there are multiple variables being assigned.

Likewise, when creating objects, it's preferable to name your variables to match the object and assign them using the literal shorthand (e.g., `var text = "hello, world"; var obj = { text };`).

Use spread instead of `Function.apply` when calling a function with multiple arguments, as it's more readable and doesn't have concerns about assigning the context object. You can also use the object spread instead of `Object.assign()` for combining objects, as it avoids accidentally mutating the initial merged object. Likewise, use `...rest` for functions that can take variable arguments instead of using the `arguments` object directly.

```js
// use destructuring when extracting items from modules or data
var { classify } = require("./lib/helpers");
var { key, value } = item;

// destructuring is useful for processing dates
var [ m, d, y ] = dateString.split("/").map(Number);

// Use spread for variadic functions, or combining objects
var min = Math.max(...values);
destinationArray.push(...newItems);

var d3 = {
  ...require("d3-axis"),
  ...require("d3-scale")
};
```

### Template strings

Never concatenate more than two strings together--use a template string instead to do interpolation. However, try to avoid doing large amounts of work inside the template string, and do not use them where a real templating engine would be a better choice (i.e., do not combine template strings with `map()` to build repeated HTML structures, especially if there are conditionals).

```js
// too much noise
var translate = "translate(" + margins.left + ", " + margins.top + ")";

// cleaner
var translate = `translate(${margins.left}, ${margins.top})`;

// cleanest
var makeTranslate = (x, y) => `translate(${x}, ${y})`;
var translate = makeTranslate(margins.left, margins.top);

```

### Modules

You can use ES modules in your code if you want--Babel will transpile it into `require()` calls for the final output. You can also use CommonJS exports. Your modules should export either an object or a function, with the former preferred so that you can pull out specific parts of the module using destructuring.

### Classes/prototypes

ES classes are supported by all browsers that we currently target, and they may be used if you are more comfortable with their code pattern than with prototypal inheritance. It's best not to build deep inheritance structures in JS.

In both classes and prototypes, only initialize properties that are reference values (objects and arrays) in the constructor function. You should not be creating methods in the constructor, as this wastes memory. Put those on the prototype or declare them in the class itself. You may, however, use the constructor to bind methods to this particular instance, if necessary.

## Libraries and patterns

Whenever possible, prefer using browser built-ins over any library. They're lighter in weight, are less likely to break, and teach more reusable patterns for novice developers. We have a collection of micro-modules for common tasks that are bigger than one-liners, [here](https://github.com/nprapps/interactive-template/tree/master/root/src/js/lib), such as performing XHR downloads or delegating events.

That said, we will use libraries or frameworks where their utility is sufficiently high to overcome their bundled size. We'll decide this on a case by case basis.

### Document manipulation (vs. jQuery/D3)

```js

// Finding elements
var $ = (s, d = document) => Array.from(d.querySelectorAll(s));
$.one = (s, d = document) => d.querySelector(s);

// Updating classes or attributes
$(".add-a-class").forEach(el => el.classList.add("selected"));
$(".set-an-attribute").forEach(el => el.setAttribute("fill", "red"));

// Adding event listeners
var onClick = function() { ... };
$(".clickable").forEach(el => el.addEventListener("click", onClick));

// Getting data attributes
var dataElement = $.one(".has-data");
var { text } = dataElement.dataset;

// Simple content setting
var name = "World";
$.one(".status").innerHTML = `Hello, ${name}`;

// Running code based on window size
var isMobile = window.matchMedia("(max-width: 500px)");
// matches property is "live", will update after resize
if (isMobile.matches) {
  // run on a phone
}

```

### Data collections (vs. Underscore/D3-array)

```js
// each
data.forEach((d, i) => doSomething(d, "argument"));

// pluck()
var plucked = data.map(d => d.property);

// unique
var unique = [...new Set(data)];

// filter
var filtered = data.filter(d => d.value == "xyz");

// min/max/extent
var values = data.forEach(d => d.value);
var min = Math.min(...values);
var max = Math.max(...values);
var sorted = values.slice().sort();
var extent = [sorted.shift(), sorted.pop()];

// Object keys
Object.keys(data).forEach(key => console.log(key));

// Un-pivoting columnar data
var skipLabels = ["label", "name", "etc"];
DATA.forEach(function(d) {
  d.values = Object.keys(d)
    .filter(key => skipLabels.indexOf(key) == -1)
    .map(function(key) {
      return {
        key,
        value: data[k]
      }
    });
});

```