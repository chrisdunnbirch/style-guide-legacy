# Coding Style Guide

## **Contents**

* [PHP](#markdown-header-php)
* [HTML](#markdown-header-html)
* [SCSS](#markdown-header-scss)

---

## **PHP**

PHP style guide here.

---

## **HTML**

HTML style guide here.

---

## **SCSS**

---

### Directory structure
Having a consistent directory structure across projects ensures that any developer working any project will be able to immediately know where to look when amendments are to be made.

A barebones version of the SCSS directory structure is included within the fresh installation repository.

```
scss/
|   _base/
|   |   _normalise.scss
|   |   _fonts.scss
|   |   _config.scss
|   |   _functions.scss
|   |   ...
|   _components/
|   |   _wp_wysiwyg.scss
|   |   _gravity_form.scss
|   |   ...
|   _global/
|   |   _header/
|   |   |   _masthead.scss
|   |   |   _navigation.scss
|   |   |   ...
|   |   _footer/
|   |   |   _navigation.scss
|   |   |   _company.scss
|   |   |   ...
|   _panels/
|   |   _hero.scss
|   |   _banner.scss
|   |   _people.scss
|   |   ...
|   _content/
|   |   _posts.scss
|   |   _post.scss
|   |   ...
|   application.scss
```

#### Base

* `_normalise.scss` does what it says on the tin and should never need to be changed.
* `_fonts.scss` should be used to declare any @font-face rules and/or import any remote web fonts.
* `_config.scss` should be used for the declaration of any relevant theme variables, including colours and standardised layout values.
* `_functions.scss` contains useful SASS mixins.

#### Components

A component partial should be created for anything that is to be standardised across the entire website, such as panel titles and buttons.

* `_wp_wysiwyg.scss` contains the class which we will use to wrap any WYSIWYG editor ouput. This allows us to define rules for anything that can be configured by clients using a WYSIWYG editor.
* `_gravity_form.scss` should be used to style Gravity Forms output.

#### Global

A global partial should be created for anything that appears in either the header or the footer. A typical design will have a separate masthead and navigation block within the header and a separate navigation and company/legal block in the footer.

#### Panels

A panel is defined as any panel within the theme which is not template-specific such as a hero panel, banner panel, people panel, etc.

Typically each panel partial will represent a single row within the 'Panels' flexible content field created using ACF.

#### Content

A content panel is defined as any panel within the theme which is template specific such as the blog posts listing panel and the individual blog post panel.

#### Application

This is the primary stylesheet in to which each partial should be imported, this should contain no other rules.

---

### Partial structure

Much like the directory structure, keeping a consistent structure within each partial will ensure developer sanity.

This applies mostly to panels and content panels partials. Each partial should be separated in to 3 clear sections, let's take a "services" panel in a desktop-first build as an example.

```scss
// variables
$service_column_width: 25%;

// default rules
.panel--services {}

// responsive rules
@media screen and (max-width: 1024px)
{
    .panel--services {}
}

@media screen and (max-width: 768px)
{
    .panel--services {}
}
```

Any partial-specific variables should be defined at the top of the stylesheet.

If the theme is being developed desktop-first, the variables should be followed by the desktop styling then any responsive rules in decreasing **max-**width order.

If the theme is being developed mobile-first, the variables should be followed by the mobile styling then any responsive rules in increasing **min-**width order.

---

### Variables

For readability, variables should be named using lowercase with underscores between words.

```scss
// bad
$ColumnWidth: 25%;
$columnWidth: 25%;
$column-width: 25%;

// good
$column_width: 25%;
```

`_config.scss` contains standard variables, with other likely-to-be-required variables commented out for ease of use.

Firstly we have greyscale values from `$white` through to `$black`. The greys are design-specific and as such are commented out in the fresh installation repository. These should follow the convention of `$grey_{shade}` where typically there will be no more than 5 greys throughout a design so we have superlight, light, mid, dark and superdark. Should the design use more than 5 grey values consider discussing if any of them can be changed to stick to a maximum of 5, however in a case where this is deemed not to be possible you should use a common-sense approach to adding more shades.

Then we have social network colours, which shouldn't need to be changed, followed by branding colours. These are project dependent and will be either `$brand_primary`, `$brand_secondary`, `$brand_tertiary` or in cases where there is no hierarchy to the use of colour within the client's brand `$brand_{colour}` such as `$brand_blue`, `$brand_red`, etc.

Finally we have standardised structural variables which should be used throughout the majority of builds. Only `$site_width` should need to be changed at the start of each project dependent upon the design, though typically ApplinSkinner designs will be 1140px.

---

### Naming conventions

IDs should only be used for elements which are to be targetted with JavaScript (jQuery) events.

Classes and IDs should be named using lowercase whilst following the [CSSWizardry variation of the BEM methodology](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/), emphasising semantics over general language such as "left", "right", "column" and "row".

```scss
.block {}
    .block__element {}
        .block__element--modifier {}
```

Contrary to CSSWizardry, in rare instances where a block or element name must contain multiple words use a single underscore (`_`) instead of using a single hyphen (`-`).

```scss
// bad
.block-name {}
    .block-name__element-name {}

// good
.block_name {}
    .block_name__element_name {}
```

---

### SCSS Nesting & Indentation

Use soft tabs with four spaces as this is the only way to guarantee code renders the same in any environment.

Nesting should be limited to pseudo-classes such as `:hover` and pseudo-selectors such as `::before` to prevent styles from becoming overly specific. As such, use indentation to mimic the readability benefits of nesting by matching the DOM such that other developers can see how each class relates to those around it in the mark-up.

```html
<div class="parent1">
    <div class="parent1__child1"></div>
    <div class="parent1__child2"></div>
</div>
<div class="parent2">
    <div class="parent2__child1"></div>
    <div class="parent2__child2"></div>
</div>
```

```scss
// bad
.parent1 {
    .parent1__child1 {}
    .parent1__child2 {}
}
.parent2 {
    .parent2__child1 {}
    .parent2__child2 {}
}

// good
.parent1 {}
    .parent1__child1 {}
    .parent1__child2 {}
.parent2 {}
    .parent2__child1 {}
    .parent2__child2 {}
```

Any elements which have property declarations dependent upon a modifier to the block should be nested within the modifier.

```scss
// bad
.parent {}
    .parent__child1 {}
    .parent__child2 {}
    .parent--modifier {}
        .parent--modifier .parent__child1 {}

// good
.parent {}
    .parent__child1 {}
    .parent__child2 {}
    .parent--modifier {
        .parent__child1 {}
    }
```

---

### Selector order

The order of selectors should follow a specific order within each block such that it is easy for a developer to identify what they're looking for.

Nested selectors should appear in the following order:

```scss
.block {
    &:empty {}
    &:not() {}
    // inputs
    &:required {}
    &:optional {}
    &:checked {}
    &:valid {}
    &:invalid {}
    &:enabled {}
    &:disabled {}
    // child
    &:only-child {}
    &:first-child {}
    &:nth-child() {}
    &:last-child {}
    &:nth-last-child() {}
    // type
    &:only-of-type {}
    &:first-of-type {}
    &:nth-of-type() {}
    &:last-of-type {}
    &:nth-last-of-type() {}
    // language
    &:lang() {}
    // blank pseudo-elements
    &::before {}
    &::after {}
    // content pseudo-elements
    &::first-letter {}
    &::first-line {}
    // selection
    &::selection {}
    // hyperlinks
    &:target {}
    &:link {}
    &:visited {}
    &:focus {}
    &:hover {}
    &:active {}
}
```

Descendent selectors should appear in the same order in which they appear in the DOM, followed by modifier selectors.

```scss
.block {}
    .block__child1 {}
    .block__child2 {}
    .block--modifier1 {}
    .block--modifier2 {}
```

---

### Declaration order

Related property declarations should be grouped together following the order:

* Positioning
* Box model
* Visual
* Typography
* CSS3

```scss
// required for pseudo-elements
content

// positioning
position
z-index
top
right
bottom
left

// box model
display
float
vertical-align
width
max-width
min-width
height
max-height
min-height
margin
margin-top
margin-right
margin-bottom
margin-left
padding
padding-top
padding-right
padding-bottom
padding-left
column-count
column-gap

// visual
background
background-color
background-image
background-repeat
background-position
background-size
color
border
border-size
border-style
border-color
border-radius
border-top
border-top-size
border-top-width
border-top-color
border-right
border-right-size
border-right-width
border-right-color
border-bottom
border-bottom-size
border-bottom-width
border-bottom-color
border-left
border-left-size
border-left-width
border-left-color
outline
outline-width
outline-style
outline-color
box-shadow
opacity
visibility

// typography
font
font-family
font-size
font-weight
font-style
line-height
text-align
text-decoration
text-transform
text-shadow

// css3
transform
transform-origin
transition
```

---

### Declaration syntax

When grouping selectors, keep individual selectors to a single line.

```scss
// bad
.block, .block__element {
    property: value;
}

// good
.block,
.block__element {
    property: value;
}
```

Either include one space after the final selector or use a new line for the opening brace of selector block for legibility. Closing braces of each selector block should always be on a new line.

```scss
// bad
.block{
    property: value;
}

// okay
.block {
    property: value;
}

// best
.block
{
    property: value;
}
```

End each declaration with a semi-colon (`;`) and include one space after the colon (`:`) with no space before it for each property declaration.

```scss
// bad
.block {
    property1:value;
    property2:value
}

// good
.block {
    property1: value;
    property2: value;
}
```

Each property declaration should appear on it's own line for legibility and debugging purposes.

```scss
// bad
.block {
    property1: value; property2: value;
}

// good
.block {
    property1: value;
    property2: value;
}
```

Comma-separated property values such as `box-shadow` should include a space after each comma.

```scss
// bad
.block {
    box-shadow: 0 1px 2px #ABC,2px 1px 0 #DEF;
}

// good
.block {
    box-shadow: 0 1px 2px #ABC, 2px 1px 0 #DEF;
}
```

Include leading 0 for decimal values and include spaces between each value for `rgb()`, `rgba()`, `hsl()`, `hsla()` and `rect()`.

```scss
// bad
.block {
    color: rgba(255,255,255,.5);
}

// good
.block {
    color: rgba(255, 255, 255, 0.5);
}
```

Ensure all hex values use uppercase characters for legibility as this standardises the height of each character when mixed with numbers.

```scss
// bad
$brand_primary: #53ec7f;

// good
$brand_primary: #53EC7F;
```

Always include quotes with attribute values within attribute selectors. These are [optional in some cases](http://mathiasbynens.be/notes/unquoted-attribute-values#css), but for consistency it is good practice to standardise.

```scss
// bad
input[type=text]

// good
input[type="text"]
```

Avoid specifying units for zero values.

```scss
// bad
.block {
    margin: 0px;
}

// good
.block {
    margin: 0;
}
```