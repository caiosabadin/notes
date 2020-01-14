# Sass #

Sass is a CSS preprocessor, which means it needs to be compiled in order to generate a .css file.
Its file extension is .scss.

## Installation ##

Install Ruby. After that, use ``gem`` to install Sass, then check if everything is alright calling it with the option ``-v``:

```bash
curl -sSL https://get.rvm.io | bash -s stable
gem install sass
sass -v
```

## Compiling ##

In order to transform a .scss file into a .css one, just run:

```bash
sass file.scss:another_file.css
```

You can use ``--watch`` in order to oblige Sass to automatically recompile whenever there are changes to the .scss file:

```bash
sass --watch file.scss:another_file.css
```

To try it online, one might use https://www.sassmeister.com/

## How to write expressions ##

Plain CSS is valid SCSS, but the opposite way round may not be true. Valid Sass looks just like this:

```scss
selector {
	property: value;
}
```

## Encoding ##

It is a recommended practice to enforce the character encoding right before anything else on the current stylesheet, by using the code:

```scss
@charset 'utf-8';
```

For other useful guidelines, refer to https://sass-guidelin.es/

## Commentaries ##

Plain CSS commentaries work, but if you want to write comments that are going to be ignored by the compiler...:

```scss
// Some comment
```

## Variables ##

In Sass, you can declare a variables are declared and might be used like this:

```scss
// This declaration means it's a variable with a value
$color_gray: #ccc;

body {
	background-color: rgb($color_gray, 0.9);
}

// This declaration means it's a string variable, to replace some code chunk:
$media_specification: "(max-width: 600px)";
```

Colour values might be written in whatever preferred format, and they might be used wherever, even with RGB or HSL functions.

Just as in any other regular programming language, Sass's variables are limited by their scopes.

## Interpolation ##

When we use such a variable as the last one mentioned above, interpolation is needed so that Sass may replace the chunks of code we want with that variable value. Simply call the variable inside ``#{$variable}``:

```scss
$media_specification: "(max-width: 600px)";

@media #{$media_specification} {
	body {
		background: white;
	}
}
```

Interpolation is also needed when you want to write a variable inside the quotes, so the previous excerpt could be improved as the follow:

```scss
$max_size: 600px;

$media_specification: "(max-width: #{$max_size})";

@media #{$media_specification} {
	body {
		background: white;
	}
}
```

## Expressions inside variables ##

Naturally, mathematical expressions are supported by the variables, and might be used accordingly:

```scss
$height: 3em * 5;
$width: 5 + 3;
$border: 10px / 3;

div {
  height:$height;
  width:#{$width}px;
  border:round($border) solid #000;
}
```
On the code above, note that only one of the numbers are required to have their measurement units explicited. Also note it is not necessary to point this information right away. However, when the unit is not evinced by the variable declaration, an interpolation must be performed when the variable is called, in order to place the measurement units after the variable.

It's possible to use sophisticated functions to work with the variable values. Refer to https://sass-lang.com/documentation/modules/math for more information about those functions.

## More useful functions ##

Many of the functions available on Sass are not limited to the processing of numbers. One of its nicest possibilities is to work, for example, with colours. Two of those functions are specially useful when working with the hovering of elements. Those functions are lighten and darken:

```scss
$default_colour: #a33efd;

a {
	color: $default_colour;
}

a:hover {
	color: lighten($default_colour, 10%);
}

a:visited {
	color: darken($default_colour, 20);
}
```

Those functions, as perceived on the previous snippet, take two parameters: the colour which is going to be either lightened or darkened and the percentage of how much so. The percent symbol is optional.

## Creating functions ##

One could also create their own functions:

```scss
// Sass considers both - (hyphen) and _ (underscore) to be the same
@function function-name($argument1, $argument2: defaultValue, $argument3...) {
	@return $argument1 * $argument2
}
```

The ``@return`` directive always causes the function to end, and all the functions must have this directive.

An argument becomes optional when it has a default value, as in the case of ``$argument2`` above.

When an argument is followed by ``...``, it is acessed inside the function as a list.

Errors and warnings may be arisen using the respective ``@error`` and ``@warn`` directives.

Sass supports flow control, loops and iteration through lists. Refer to https://sass-lang.com/documentation/at-rules/control for a more detailed view on these.

## Nesting ##

For us not to repeat code, it's possible to nest expressions with Sass. So, for example, the last excerpt might be rewritten as:

```scss
$default_colour: #a33efd;

a {
	color: $default_colour;

	&:hover {
		color: lighten($default_colour, 10);
	}

	&:visited {
		color: darken($default_colour, 20);
  }
}
```

Hadn't the "&" been used, or had the same selector been used again inside the nesting, some weird results would have appeared in the compiled CSS file, such as:

```scss
/* This is what happens when we do not use any selector inside the nesting */
p {
  color: #a33efd;
}
p :hover {
  color: #bb70fe;
}

/* This is what happens when we use the same selector twice */
p {
  color: #a33efd;
}
p p:hover {
  color: #8b0cfc;
}
```

Needless to say, both wouldn't work expectedly. Nevertheless, nesting selectors inside each other, as in the previous second example given, is useful when we have a structure similar as the following:

```scss
#menu { ... }
#menu ul { ... }
#menu ul li { ... }
#menu ul li a { ... }
#menu ul li a:hover { ... }
```

Unless mandatory or expressly needed, it's not recommended to apply many selectors at once, in that it greatly reduces performance, since most browsers would read those selectors from left to right.

However, a structure such as above might be accordingly nested as:

```scss
$default_colour:#fff;

#menu {
	color: $default_colour;

	ul {
		list-style: none;

		li {
			padding: 5px;

			a {
				color: darken($default_colour, 100);
				
				&:hover {
					color: darken($default_colour, 85); 
				} 
			}
		}
	}
}
```

As aforesaid, the nesting technique, notably when a profoundly descending nest exists, should be employed using caution; it lessens the loading speed, and results in an increased coupling between the HTML and the CSS, causing the last to be extremely dependant on the first; thus, a small change within the HTML could cause the whole layout to crash.

## The import directive ##

Instead of using nested code, a better approach to logically divide the stylesheet on Sass is to physically split the files, lately joining them together again by the use of the extended CSS ``@import`` directive. By doing so, the number of HTTP requests are also reduced, since the compiler links all the imported files together in one.

There may be a single .scss file for each relevant part of the layout, such as the header, the footer, an about section. Since variables are limited by their scopes, it might be also interesting to have a stylesheet containing only the global variables. The same is true for other specific Sass elements, such as mixins, functions, and other assumptions that are going to be discussed eventually on this page.

So, let a layout have files such as ``variables.scss``, ``overall.scss``, ``header.scss``, ``menu.scss``, ``content.scss``, ``about.scss``, ``footer.scss``. A file such as ``style.css`` can be created, joining them all together:

```scss
@import "path/to/variables"
@import "path/to/overall"
@import "path/to/header"
@import "path/to/menu"
@import "path/to/content"
@import "path/to/about"
@import "path/to/footer"
```

The .scss extension is optional. Any other extension used results on the Sass preprocessor calling the traditional CSS ``@import`` directive, which should be avoided because of the low performance.

Anyway, since every .css file is also a .scss file, each of the project's stylesheet may be renamed as .scss and then regularly imported by the Sass compiler.

There has just been released a new functionality to Sass, called the Module System, which was crafted so that the limitations of the ``@import`` directive could be overcome. Refer to https://sass-lang.com/blog/the-module-system-is-launched for more instructions about it, since this paper is project to be a brief overview.

## Extending ##

When repeating small structures, the ``@import`` directive is not that useful. For repetitive smaller pieces of code, Sass offers two different possible resources to be employed: the mixin, whose use is going to be discussed only on the next section, and the extend.

The later might be used for cases when it's interesting for a class to have its own properties and also some other's, notably for semantic purposes, in cases such as when you have a ``button`` class and a ``button-delete`` or perhaps a ``button-save`` class, for example:

```scss
.button {
	border-radius:100%:
}
.button-delete {
	color:#f00;
}
.button-save {
	color:#0f0;
}
```

The stylesheet above may be rewritten as:

```scss
.button {
	border-radius:100%;

	&-delete {
		@extend .button;
		color:#f00;
	}

	&-save {
		@extend .button;
		color:#0f0;
	}
}
```

And then, after compiling it with Sass, we are going to obtain the result below:

```scss
.button, .button-save, .button-delete {
  border-radius: 100%;
}
.button-delete {
  color: #f00;
}
.button-save {
  color: #0f0;
}
```

Note that, when ``@extend`` is used, sub-selectors such as ``:hover`` are also extended, so the following code:

```scss
.error {
  color:#000;
}

.error:hover {
  background-color: #fee;
}

.error--serious {
  @extend .error;
  border-width: 3px;
}
```

Produces:

```scss
.error, .error--serious {
  color: #000;
}

.error:hover, .error--serious:hover {
  background-color: #fee;
}

.error--serious {
  border-width: 3px;
}
```

Naturally, due to its intrinsic characteristics, ``@extend`` may be used to lessen the code's lines, by grouping all the selectors with similar properties together in one declaration one, since a SCSS like that:

```scss
.color-black {
	color:#000;
}

.some-div {
	@extend .color-black;
	background:#eee;
}

.div {
	@extend .color-black;
	background:#aaa;
}

.another-div {
	@extend .color-black;
	background:#fff;
}
```

Would result in a CSS as the following, where each element that must have a black text colour are grouped together so that the property might be applied to all of them:

```scss
.color-black, .another-div, .div, .some-div {
  color: #000;
}

.some-div {
  background: #eee;
}

.div {
  background: #aaa;
}

.another-div {
  background: #fff;
}
```

In spite of its usefulness, this second method implies the creation of a common selector (on the previous case, ``.color-black``) that may be useless throughout the rest of the stylesheet or to the mark-up, just so the others selectors might share the same properties.

## Placeholders ##

So, to avoid building one-time-only selectors, placeholders might be applied. Placeholder selectors simply began with a per cent symbol. Its contents are always discarded by the compiler, hence never showing in the final CSS.

This way, placeholder selectors can be used to overcome the problem aforementioned on the previous section, i.e., the creation of extendable one-time-only selectors:

```scss
%color-black {
	color:#000;
}

.some-div {
	@extend %color-black;
	background:#eee;
}

.div {
	@extend %color-black;
	background:#aaa;
}

.another-div {
	@extend %color-black;
	background:#fff;
}
```

Would result in:

```scss
.another-div, .div, .some-div {
  color: #000;
}

.some-div {
  background: #eee;
}

.div {
  background: #aaa;
}

.another-div {
  background: #fff;
}
```

## Mixins ##

Mixins are another great way to practice the DRY methodology. They differ from the extend in that they don't join all the extended selectors at the beginning of the resulting CSS file, but rather replace its call by the supposed code. So, let the preprocessor work on the following code:

```scss
$radius: 25%;
@mixin round-border {
    -webkit-border-radius: $radius;
       -moz-border-radius: $radius;
            border-radius: $radius;
}

.button {
    @include round-border;
}

.some-div {
    @include round-border;
}

.another-div {
    @include round-border;
}
```

And it would give us this CSS:

```scss
.button {
  -webkit-border-radius: 25%;
  -moz-border-radius: 25%;
  border-radius: 25%;
}

.some-div {
  -webkit-border-radius: 25%;
  -moz-border-radius: 25%;
  border-radius: 25%;
}

.another-div {
  -webkit-border-radius: 25%;
  -moz-border-radius: 25%;
  border-radius: 25%;
}
```

It is also possible to pass parameters to the mixins, and set default values to those:

```scss
@mixin round-border($radius: 25%) {
    -webkit-border-radius: $radius;
       -moz-border-radius: $radius;
            border-radius: $radius;
}

.button {
    @include round-border;
}

.some-completely-rounded-div {
    @include round-border(100%);
}
```

## Either extend or mixin ##

Since both directives seem to provide equal results, it's natural to wonder what should be used.

However, it's clear when to apply either of those: if the code is to be repeated with different values at different times, the better answer are the mixins. After all, the ``@extend`` directive doesn't support parameters or default values.

Caution must be also used when it comes to the ``@extend`` because its rules are set right at the beginning of the resulting stylesheet, which might raise issues lately, since following rules may inadvertently overwrite those first ones.

Nevertheless, applying mixins in a large scale might as well be not good, in that it would repeat lines that could otherwise be joined together in a single-line group of selectors.

So, whether parameters aren't needed and one can guarantee the rules aren't going to be overwritten later, the extend technique might be employed in order to avoid crafting much too large hence heavy CSS files.

---
FALTA:
	* functions
	* if, else, for, foreach, sassscript
	* media-queries
