CSS coding style
=====================

This document defines formatting and style rules for CSS. It aims at improving collaboration, code quality, and enabling supporting infrastructure.

## 1. General principles

* Don't try to prematurely optimize your code; keep it readable and understandable.
* All code in any code-base should look like a single person typed it, even when many people are contributing to it.
* Strictly enforce the agreed-upon style.
* If in doubt when deciding upon a style use existing common patterns.

## 2. Whitespace

* Use 4 spaces for indents.
* Never mix spaces and tabs for indentation.
* Remove end-of-line whitespace.

## 3. Comments

* Place comments on a new line above their subject.
* Keep line-length to a sensible maximum, e.g., 80 columns.
* Place only short comments after a declaration. If comment is too long use shortcuts and place the comment above ruleset

```css

/* Allow only vertical resizing of textareas. */

textarea {
    resize: vertical;
}

/*
Clearfix: contain floats

For modern browsers

1. The space content is one way to avoid an Opera bug when the
`contenteditable` attribute is included anywhere else in the document.
Otherwise it causes space to appear at the top and bottom of elements
that receive the `clearfix` class.

2. The use of `table` rather than `block` is only necessary if using
`:before` to contain the top-margins of child elements.
*/

.clearfix:before,
.clearfix:after {
    content: " "; /* 1 */
    display: table; /* 2 */
}

.clearfix:after {
    clear: both;
}

```


## 4. Format

The chosen code format must ensure that code is: easy to read; easy to clearly comment; minimizes the chance of accidentally introducing errors; and results in useful diffs and blames.

### Anatomy of rulesets

```
[selector],
[selector] {
    [property]: [value];
    [<- Declaration -->]
}
```

* Use one discrete selector per line in multi-selector rulesets.
* Include a single space before the opening brace of a ruleset.
* Include one declaration per line in a declaration block.
* Use one level of indentation for each declaration.
* Indent vendor prefixed declarations so that their values are aligned.
* Include a single space after the colon of a declaration.
* Use lowercase and shorthand hex values, e.g., `#aaa`.
* Use double quotes, e.g., `content: ""`.
* Quote attribute values in selectors, e.g., `[type="checkbox"]`.
* Where allowed, avoid specifying units for zero-values, e.g., `margin: 0`.
* Include a space after each comma in comma-separated property or function values, e.g. `font-family: helvetica, arial, sans-serif;`.
* Include a semi-colon at the end of the last declaration in a declaration block.
* Place the closing brace of a ruleset in the same column as the first character of the ruleset.
* Separate each ruleset by a blank line.

```css

.selector-1,
.selector-2,
.selector-3[type="text"] {
    -webkit-box-sizing: border-box;
       -moz-box-sizing: border-box;
            box-sizing: border-box;
    display: block;
    font-family: helvetica, arial, sans-serif;
    color: #333;
    background: #fff;
    background: linear-gradient(#fff, rgba(0, 0, 0, 0.8));
}

.selector-a,
.selector-b {
    padding: 10px;
}

```

### Exceptions and slight deviations

Large blocks of repetitive declarations can use a slightly different, single-line format to make it easier to compare similar rules. In this case, a space should be included after the opening brace and before the closing brace.

```css

.ico {
    display: inline-block;
    background: url("ico-accout-sprite.png") top left no-repeat;
}

.ico-hint    { width:20px; height:17px; background-position:0 0; }
.ico-notify  { width:39px; height:39px; background-position:0 -17px; }
.ico-warning { width:31px; height:29px; background-position:0 -56px; }
.ico-print   { width:13px; height:15px; background-position:0 -85px; }
.ico-logout  { width:14px; height:20px; background-position:0 -100px; }

```

Long, comma-separated property values — such as collections of gradients or shadows — can be arranged across multiple lines in an effort to improve readability and produce more useful diffs. There are various formats that could be used; one example is shown below.

```css

.selector {
    background-image:
        linear-gradient(#fff, #ccc),
        linear-gradient(#f3c, #4ec);
    box-shadow:
        1px 1px 1px #000,
        2px 2px 1px 1px #ccc inset;
}

```

## 5. Declaration order

Related property declarations should be grouped together following the order:

1. Positioning
2. Display & Box model
3. Typographic
4. Visual
5. ...

Positioning comes first because it can remove an element from the normal flow of the document and override box model related styles. The box model comes next as it dictates a component's dimensions and placement. Definitions in box model section ordered from outer to inner, e.g. `margin — border — padding — width/height`.

Everything else takes place inside the component or without impacting the previous two sections, and thus they come last.

```css

.declaration-order {
    /* Positioning */
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 100;

    /* Box-model */
    display: block;
    float: right;
    margin: 10px;
    padding: 20px;
    width: 100px;
    height: 100px;

    /* Typography */
    font: normal 13px "Helvetica Neue", sans-serif;
    line-height: 1.5;
    text-align: center;

    /* Visual */
    color: #333;
    background-color: #f5f5f5;
    border: 1px solid #e5e5e5;
    border-radius: 3px;
    box-shadow: 0 0 3px rgba(0,0,0,0.15);

    /* Misc */
    opacity: 1;
    -webkit-transition: 0.5s;
            transition: 0.5s;
}

```

### Exceptions and slight deviations

Some defenitions may come in different sections depending on situation.

For example, sometimes `border` can affect elements dimentions, sometimes not.

```css

/* 1. Border-width affects dimmentions */

.sample-1 {
    /* Box-model */
    margin: 10px;
    border: 5px solid #e5e5e5; /* 1 */
    padding: 10px;
    width: 50%;
}

/* 
1. Border-width doesn't affects dimmentions,
It's more visual than box-model

2. Padding is no more affects dimmentions,
*/

.sample-2 {
    /* Box-model */
    -webkit-box-sizing: border-box;
       -moz-box-sizing: border-box;
            box-sizing: border-box;
    margin: 10px;
    width: 50%;
    padding: 10px; /* 2 */

    /* Visual */
    border: 5px solid #e5e5e5; /* 1 */
}

```

Another example, when typography definitions may apply for layouting:

```css

/* 1. Text-align used for layouting inner elements */

.sample-3-outer {
  display: block;
  text-align: justify; /* 1 */
}

/* 2. Vertical-align affects layout, so it is box-model definition, not typography */

.sample-3-inner {
  display: inline-block;
  vertical-align: middle; /* 2 */
}

```

## 6. Selectors

## 7. Naming conventions
