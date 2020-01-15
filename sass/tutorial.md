# Sass #

Sass is a CSS preprocessor.

Its file extension is .scss. The compilation of that file generates a .css file.

It provides a set of labour-saving directives and definitions, to help the designers write semantically valid CSS faster.

## Installation ##

Install Ruby. Then use ``gem`` to install Sass:

```bash
curl -sSL https://get.rvm.io | bash -s stable
gem install sass
```

Call Sass with the argument ``-v`` to check if everything with the installation went right:

```bash
sass -v
```

## Compiling ##

In order to compile SCSS into CSS, run:

```bash
sass source_file.scss:resulting_file.css
```

Calling it with the optional argument ``--watch`` obliges Sass to automatically recompile whenever changes are made on the specified .scss file:

```bash
sass --watch source_file.scss:resulting_file.css
```

For more information about that argument, read [http://sassbreak.com/watch-your-sass/](this article by Guil Hernandez).

Should you need to run quick tests elsewhere, there are also in-browser compilators available, such as [https://www.sassmeister.com/](SassMeister).

## Writing SCSS ##

Plain CSS is valid SCSS, although the contrary affirmation is not always true.

Therefore, the Sass syntax may be as simple as:

```scss
selector {
	property: value;
}
```

## Encoding ##

It is good practice to enforce the character encoding.

Do that right before anything else on the stylesheet:

```scss
@charset 'utf-8';
```

For other helpful guidelines, refer to [https://sass-guidelin.es/](this page by Hugo Giraudel).

## Comments ##

Plain CSS comments work on SCSS, and are written to the resulting CSS file.

For the comments not to be shown in the compiled stylesheet, they must be written as:

```scss
@charset 'utf-8';
// Some comment
```

## Variables ##

After the processing of SCSS files, the variables throughout the code are replaced by the values they are set with.

Those values range from numeric definitions to whole strings, also including hexadecimal colour expressions and beyond.

They are declared as ``$variable: value;`` and called simply by ``$variable``, e.g.:

```scss
@charset 'utf-8';
$color_gray: #ccc;

body {
	background-color: rgb($color_gray, 0.9);
}
```

Note that colours may be expressed in whatever preferred format, and that they may be used wherever, even inside RGB or HSL functions, like the example above.

Just as with many other programming languages, variables on Sass are limited by their scopes.

## Interpolation ##

Interpolation is needed when using string variables, so that Sass may replace the chunks of code with the values wanted.

To do that, call the variable inside ``#{$variable}``:

```scss
@charset 'utf-8';
$media_specification: "(max-width: 600px)";

@media #{$media_specification} {
	body {
		background: white;
	}
}
```

Interpolation is also necessary when a variable is called inside quotes: 

```scss
@charset 'utf-8';
$max_size: 600px;
$media_specification: "(max-width: #{$max_size})";

@media #{$media_specification} {
	body {
		background: white;
	}
}
```

## Expressions inside variables ##

Variables work with mathematical expressions, and they also support sophisticated function calls.

The result of those expressions or functions replaces the variable wherever it has been called.

Only a single number in the expression is required to have explicit measurement units. The others are logically deduced based solely on this one.

When the expression evinces no unit right away, an interpolation must be performed to call the variable, making it possible to specificy its unit by its side.

Hence, the rules below:

```scss
@charset 'utf-8';
$border: round(10px / 3px);
$height: 3em * 5;
$width: 5 + 3;

div {
  border:$border solid #000;
  height:$height;
  width:#{$width}px;
}
```

Prompt this:

```scss
div {
  border: 3px solid #000;
  height: 15em;
  width: 8px;
}
```

For more information about mathematical functions such as ``round()``, refer to [https://sass-lang.com/documentation/modules/math](the official documentation).

## Creating functions ##

Sass permits the creation of functions:

```scss
@charset 'utf-8';

@function function-name($argument1, $argument2: defaultValue, $argument3...) {
	$result: $argument1 + $argument2;
	@each $number in $argument3 {
		$result: $result * $number;
	}
	@return $result;
}
```

Functions have an obligatory ``@return`` directive. It causes the function to end when it is reached.

Arguments become optional when a default value is defined to them. It is the case of the ``$argument2`` above.

If an argument is followed by ``...``, it must be acessed inside the function as a list, like ``$argument3`` was, on the previous code.

Errors and warnings may be arisen when the respective ``@error`` and ``@warn`` directives are applied.

When calling the function, both hyphen and underscore are considered the same character.

Sass supports flow control, loops and iteration through lists. Refer to its [https://sass-lang.com/documentation/at-rules/control](official documentation) for a detailed view on these.

## More useful functions ##

Besides the processing of numbers, the built-in functions on Sass also deal with strings, colours and more.

For example, ``lighten()`` and ``darken()`` causes a colour to respectively turns lighter or darker according to a specified percentage, whose symbol is optional:

```scss
@charset 'utf-8';
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

VER A PARTIR DA AQUI
## Nesting ##

For us not to repeat code, it's possible to nest expressions with Sass. So, for example, the last excerpt might be rewritten as:

```scss
@charset 'utf-8';
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
@charset 'utf-8';
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

## Media Queries ##

Media queries might be nested as well, inside the selectors that should be modified by them:

```scss
@charset 'utf-8';
$media_specification: "(max-width: 600px)";

header {
  font-size:2rem;
  
  @media #{$media_specification} {
    font-size:3rem;
  }
}
```

Which would result in:

```css
header {
  font-size: 2rem;
}
@media (max-width: 600px) {
  header {
    font-size: 3rem;
  }
}
```

## The import directive ##

Instead of using nested code, a better approach to logically divide the stylesheet on Sass is to physically split the files, lately joining them together again by the use of the extended CSS ``@import`` directive. By doing so, the number of HTTP requests are also reduced, since the compiler links all the imported files together in one.

There may be a single .scss file for each relevant part of the layout, such as the header, the footer, an about section. Since variables are limited by their scopes, it might be also interesting to have a stylesheet containing only the global variables. The same is true for other specific Sass elements, such as mixins, functions, and other assumptions that are going to be discussed eventually on this page.

So, let a layout have files such as ``variables.scss``, ``overall.scss``, ``header.scss``, ``menu.scss``, ``content.scss``, ``about.scss``, ``footer.scss``. A file such as ``style.css`` can be created, joining them all together:

```scss
@charset 'utf-8';
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
@charset 'utf-8';
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
@charset 'utf-8';
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
@charset 'utf-8';
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
@charset 'utf-8';
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
@charset 'utf-8';
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
@charset 'utf-8';
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

## What else to do in Sass? ##

Besides the usual reading about the different frameworks derivated from Sass, such as [http://compass-style.org/](Compass), there is more to check.

It is recommended learning how to work with the [https://sass-lang.com/documentation/at-rules/use](the new modules system), which should replace most, if not all, uses of ``@import``.

Getting to know more about SassScript may be as well very benefic. And, to do that, being acquainted with [https://sass-lang.com/documentation/at-rules/control](the flow control rules) is imperative.

Last but not least, the awareness of [https://sass-lang.com/documentation/modules](some of its built-in modules' functions) may someday prove very useful, specially those related to [https://sass-lang.com/documentation/modules/math](mathematics), [https://sass-lang.com/documentation/modules/color](colours), [https://sass-lang.com/documentation/modules/string](strings), and [https://sass-lang.com/documentation/modules/list](lists).
