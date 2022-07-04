# Sass

https://sass-lang.com/guide
https://sass-lang.com/documentation/
https://www.w3schools.com/sass/

## installation:

```
npm install -g sass
```

## usage:

```
sass --watch <input_file.scss> <output_file.css>
```

## variables:

- Sass variables are only available at the level of nesting where they are defined.

- `!global` indicates that a variable is global, which means that it is accessible on all levels.

- Shadowning: Local variables can even be declared with the same name as a global variable. If this happens, there are actually two different variables with the same name: one local and one global.

- `!default` This assigns a value to a variable only if that variable isn’t defined or its value is null. Otherwise, the existing value will be used.

- Variables that are defined by a built-in module cannot be modified.

```scss
@use "sass:math" as math;
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

$color: red;
$color: blue !default; // this will not override $color
math.$pi: 0; // this assignment will fail

body {
  font: 100% $font-stack;
  color: $primary-color;
}
h1 {
  $myColor: green !global; // this mean that $myColor is accessible on all levels
  color: $myColor;
}

p {
  color: $myColor;
}
```

## Nesting:

```scss
.outer {
  .inner {
    color: red;
  }
}
```

## Partials:

```scss
// _base.scss
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

```scss
// styles.scss
@import "base";

.inverse {
  background-color: $primary-color;
  color: white;
}
```

```scss
@use "partial.scss";
```

## Modules (namespaces):

```scss
// _base.scss
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

```scss
// styles.scss
@use "base"; // not @import "base";

.inverse {
  background-color: base.$primary-color; // not $primary-color;
  color: white;
}
```

## Mixins:

- A mixin lets you make groups of CSS declarations that you want to reuse throughout your site.
- . You can pass in values to make your mixin more flexible.

```scss
@mixin theme($theme: DarkGray) {
  background: $theme;
  box-shadow: 0 0 1px rgba($theme, 0.25);
  color: #fff;
}

.info {
  @include theme;
}
.alert {
  @include theme($theme: DarkRed);
}
.success {
  @include theme($theme: DarkGreen);
}
```

## Extend/Inheritance:

```scss
/* This CSS will print because %message-shared is extended. */
%message-shared {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

// This CSS won't print because %equal-heights is never extended.
%equal-heights {
  display: flex;
  flex-wrap: wrap;
}

.message {
  @extend %message-shared;
}
```

## Operators:

```scss
@use "sass:math";

.container {
  display: flex;
}

article[role="main"] {
  width: math.div(600px, 960px) * 100%;
}

aside[role="complementary"] {
  width: math.div(300px, 960px) * 100%;
  margin-left: auto;
}
```

## @if and @else

```scss
@use "sass:math";

@mixin triangle($size, $color, $direction) {
  height: 0;
  width: 0;

  border-color: transparent;
  border-style: solid;
  border-width: math.div($size, 2);

  @if $direction == up {
    border-bottom-color: $color;
  } @else if $direction == right {
    border-left-color: $color;
  } @else if $direction == down {
    border-top-color: $color;
  } @else if $direction == left {
    border-right-color: $color;
  } @else {
    @error "Unknown direction #{$direction}.";
  }
}

.next {
  @include triangle(5px, black, right);
}
```

## @each

```scss
// list
$colors-list: red, green, blue;
@each $color in $colors-list {
  .text-#{$color} {
    color: $color;
  }
}

// map
$colors-map: (
  red: red,
  green: green,
  blue: blue,
);
@each $color, $value in $colors-map {
  .bg-#{$color} {
    background-color: $value;
  }
}

// Destructuring (list of lists)
$icons: "eye" "\f112"12px, "start" "\f12e"16px, "stop" "\f12f"10px;

@each $name, $glyph, $size in $icons {
  .icon-#{$name}:before {
    display: inline-block;
    font-family: "Icon Font";
    content: $glyph;
    font-size: $size;
  }
}
```

## @for

<stong>Syntax:</strong>

- @for <variable> from <expression> to <expression> { ... }
- @for <variable> from <expression> through <expression> { ... }

```scss
$base-color: #036;

@for $i from 1 through 3 {
  ul:nth-child(3n + #{$i}) {
    background-color: lighten($base-color, $i * 5%);
  }
}
```

## @while

<stong>Syntax:</strong>

- @while <expression> { ... }

```scss
@use "sass:math";

/// Divides `$value` by `$ratio` until it's below `$base`.
@function scale-below($value, $base, $ratio: 1.618) {
  @while $value > $base {
    $value: math.div($value, $ratio);
  }
  @return $value;
}

$normal-font-size: 16px;
sup {
  font-size: scale-below(20px, 16px);
}
```

<strong>⚠️ Heads up!</strong>
Heads up!
Some languages consider more values falsey than just false and null. Sass isn’t one of those languages! Empty strings, empty lists, and the number 0 are all truthy in Sass.
