# Sass #

Sass is a CSS preprocessor.

Its file extension is .scss. The compilation of that file generates a .css file.

It provides a set of labour-saving directives, instructions and definitions, to help the designers write semantically valid CSS faster.

## Index ##

1. [Installation](#Installation)
2. [Compiling](#Compiling)
2. [Syntax](#Syntax)
2. [Encoding](#Encoding)
2. [Comments](#Comments)
2. [Placeholders](#Placeholders)
2. [Variables](#Variables)
2. [Interpolation](#Interpolation)
2. [Expressions](#Expressions)
2. [Functions](#Functions)
2. [Colours](#Colours)
2. [Nesting](#Nesting)
2. [Media Queries](#Media-Queries)
2. [Importing](#Importing)
2. [Extending](#Extending)
2. [Mixins](#Mixins)
2. [Content](#Content)
2. [Either/Or](#eitheror)
2. [Furthering](#Furthering)

## Installation ##

Download your version [from here](https://github.com/sass/dart-sass/releases/tag/1.25.0).

Either add it to your PATH variable or create a symbolic link.

Call Sass with the argument ``-v`` to check if everything with the installation went right:

```bash
sass -v
```

## Compiling ##

In order to compile SCSS into CSS, run:

```bash
sass source_file.scss:resulting_file.css
```

Calling it with the optional argument ``--watch`` obliges Sass to automatically recompile whenever changes are made to the specified .scss file:

```bash
sass --watch source_file.scss:resulting_file.css
```

For more information about that argument, read [this article by Guil Hernandez](http://sassbreak.com/watch-your-sass).

Should a quick test be run elsewhere, in-browser compilators are available, such as [SassMeister](https://www.sassmeister.com).

## Syntax ##

Plain CSS is valid SCSS, although the contrary affirmation is not always true.

Therefore, the Sass syntax may be as simple as:

```scss
selector {
    property: value;
}
```
There is another syntax available. To work with it, files must be saved as .sass instead of .scss.

In the same manner of Ruby or Python, blocks are not demarcated by curly brackets, solely by identation, and no semicolon is placed at the ending of the properties' declaration.

```sass
selector
    property: value
    property: value
    
selector
    property: value
```

However, this format breaks compatibility with regular CSS files, a problem not of little consequence.

In effect, it immediately makes copy and paste harder and, in the long run, other possible issues may arise, such as flawing the use of [the ``@import`` directive](#Importing).

In the light of what was discussed, this older syntax is thus advised against.

## Encoding ##

It is good practice to enforce character encoding.

Put this right before anything else on the stylesheet:

```scss
@charset 'utf-8';
```

For other helpful guidelines, refer to [this page by Hugo Giraudel](https://sass-guidelin.es/).

## Comments ##

Plain CSS comments work on SCSS, and are written to the resulting CSS file.

For the comments not to be shown in the compiled stylesheet, they must be written as:

```scss
@charset 'utf-8';

// Some comment
```

## Placeholders ##

Placeholders are special selectors preceded by a percent symbol.

Their content is always discarded by the compiler, hence never appearing in the final CSS:

```scss
@charset 'utf-8';

// This selector is not going to be shown elsewhere:
%selector {
    color: #000;
}
```

Notwithstanding their apparent fruitlessness, there are certain situations whose only optimal solution would be the use of placeholders. For instance, they are specially handy when [extending selectors](#extending), a situation yet to be addressed by this guide.

## Variables ##

A variable holds values. They may be: numeric, list, strings, hexadecimal colours, among others.

After the processing of a SCSS file, variables are thoroughly replaced by the values they were set with.

They are similarly declared as regular CSS properties, except a dollar symbol precedes their name, i.e. ``$variable: value;``. They are called by ``$variable``, e.g.:

```scss
@charset 'utf-8';

$color_gray: #ccc;

body {
    background-color: rgb($color_gray, 0.9);
}
```

Note that colours may be expressed in whatever preferred format, and that they may be used wherever, even inside RGB or HSL functions.

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

## Expressions ##

Variables work with mathematical expressions, and they also support some sophisticated function calls.

Results of those expressions and functions replace the variable wherever it has been called.

The rules below:

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

Prompt:

```scss
div {
  border: 3px solid #000;
  height: 15em;
  width: 8px;
}
```

Only a single number in the expression is required to have explicit measurement units. The others are logically deduced based on this sole one.

When the expression evinces no unit right away, an interpolation must be performed in order to call the variable, letting its unit to be specified beside it.

For more information about mathematical functions such as ``round()``, refer to [the official documentation](https://sass-lang.com/documentation/modules/math).

## Functions ##

Sass permits the creation of functions:

```scss
@charset 'utf-8';

@function function-name($argument1, $argument2: defaultValue, $argument3...) {
    $resultult: $argument1 + $argument2;
    @each $number in $argument3 {
        $resultult: $resultult * $number;
    }
    @return $resultult;
}
```

Functions have an obligatory ``@return`` directive. It causes the function to end when it is reached.

Arguments become optional when a default value is defined to them. It is the case of the ``$argument2`` above.

If an argument is followed by ``...``, it must be acessed inside the function as a list, like ``$argument3`` was, on the previous code.

Errors and warnings may be arisen by applying the respective ``@error`` and ``@warn`` directives.

Both hyphen and underscore are considered the same character when calling the function.

Sass supports flow control, loops and iteration through lists. For a detailed view on these, check [its official documentation](https://sass-lang.com/documentation/at-rules/control).

## Colours ##

Besides the processing of numbers, the built-in functions on Sass also deal with [strings](https://sass-lang.com/documentation/modules/string), [colours](https://sass-lang.com/documentation/modules/color) and [more](https://sass-lang.com/documentation/modules).

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

Since both ``lighten()`` and ``darken()`` have a low transformation threshold, rapidly turning the desired colour into white and black respectively, [Giraudel](https://sass-guidelin.es/#lightening-and-darkening-colors) advises against their adoption, rather recommending the employing of the smoother ``mix()`` function:

```scss
@charset 'utf-8';

$default_colour: #a33efd;

a {
    color: $default_colour;
}

a:hover {
    color: mix(white, $default_colour, 10%);
}

a:visited {
    color: mix(black, $default_colour, 20);
}
```

## Nesting ##

Nesting the selectors makes it possible not to repeat code unnecessarily.

They are specially convenient when a selector holds a significant number of pseudo-classes, which was the case of the last excerpt from the previous section. This may be rewritten, using the nesting technique, as:

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

In the resulting stylesheet, the ampersand selector is replaced by the higher-level selector it is nested within.

Had not the ampersand been used, or had the same selector been used again inside the nesting, the results would not have  worked expectedly:

```scss
/* This is what happens when no selector is applied inside the nesting */
p {
  color: #a33efd;
}
p :hover {
  color: #bb70fe;
}

/* This is what happens when the ampersand is replaced by the higher-level selector */
p {
  color: #a33efd;
}
p p:hover {
  color: #8b0cfc;
}
```

However, nesting selectors inside each other is useful for repetitive structures similar as:

```scss
#menu { ... }
#menu ul { ... }
#menu ul li { ... }
#menu ul li a { ... }
#menu ul li a:hover { ... }
```

Which may be accordingly nested as:

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

Readers are cautioned against nesting, unless the resulting structure is mandatory or expressly needed, in that it greatly reduces performance: nestings could go fathomlessly, and the greater the number of selectors, the slower the browser interprets the rules specified by them, since most browsers would read those selectors from left to right.

Moreover, nesting may also increase coupling between the HTML and the CSS, causing the last to be extremely dependant on the former, thus rising the possibility of a whole layout crash after minor changes to the HTML.

## Media Queries ##

Media queries can be nested inside the selectors they must adjust.

This way, the query stays closer to the element they modify, improving readability:

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

Anyhow, the community discourage designers from doing so, since Sass does not group identical rules together in a single declaration; instead, it repeats them all over the code.

Nevertheless, [on this text](https://medium.com/front-end-developers/the-solution-to-media-queries-in-sass-5493ebe16844), Dominique Briggs arranged a simple yet elegant solution to this issue.

## Importing ##

Instead of using nested code, a better approach to logically divide the stylesheet is to physically split its related parts into separate files.

Those files are compiled and then regrouped into a single one again by using the the Sass-extended ``@import`` directive, in order to lessen the number of HTTP requests made to the server.

So, supposing a layout have files such as ``variables.scss``, ``overall.scss``, ``header.scss``, ``menu.scss``, ``content.scss``, ``about.scss``, ``footer.scss``, a file named ``style.css`` could be created as the following, combining all the others altogether:

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

The .scss extension can be omitted. Any other extension used is interpreted by Sass as a call to the traditional CSS ``@import`` directive, which should be avoided because of its low throughput. Albeit it might seem as an issue, this can be dealt with: since every CSS is also SCSS, any stylesheet may be renamed as .scss and then be imported by Sass regularly.

A new functionality has been added to Sass recently. It is called the Module System, created to overcome ``@import`` limitations. For more instructions about it, [refer to the official documentation](https://sass-lang.com/blog/the-module-system-is-launched).

## Extending ##

Extending is one of the many forms Sass implemented the DRY methodology.

It is used when it is interesting to share CSS rules with many different selectors at once.

This lessens the number of lines, in that it groups all those selectors with similar properties in single declarations, at the beginning of the CSS file. Therefore, a SCSS like:

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

Would result in a CSS as the following, where each element that must have a black text colour are grouped together so that the property is applied to all of them:

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

Despite its usefulness, this method sometimes implies the creation of a common selector (``.color-black`` on the previous case) that may be useless throughout the rest of the stylesheet or the mark-up, just so the others selectors might share its properties.

Placeholders may be applied to fix this one-time-only selectors issue:

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

Note that sub-selectors such as ``:hover`` are also extended, which may lead to unpredicted results oblivious users are particularly susceptible to.

So, the code below:

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

Generates:

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

Extending is distinctively advantageous to the implementation of [the BEM methodology](http://getbem.com/naming/), in that ``@extend`` assures both the classes and their modifiers are going to share styles, thereby saving the mark-up from the exaggerated number of classes that would otherwise be necessary in order to follow BEM.

Therefore, instead of giving as many classes as ``block block-lg block-lg-red`` to a HTML element, a single ``block-lg-red`` would work, provided that the SCSS is written as below:

```scss
@charset 'utf-8';

.block {
    display: block;
    
    &-lg {
        @extend .block;
        width:800px;
    
        &-red {
            @extend .block-lg;
            background-color:#f00;
        }
    }
}
```

Because its compiled version would be:

```css
.block, .block-lg, .blok-lg-red {
  display: block;
}

.block-lg, .blok-lg-red {
  width: 800px;
}

.blok-lg-red {
  background-color: #f00;
}
```

## Mixins ##

Mixins enable reuse of style definitions.

They resemble variables to some extent. The most significant difference is that whilst variables carry values, mixins carry styles excerpts.

They are declared with the ``@mixin`` directive. After their declaration, they are later called with the ``@include`` directive.

After the processing, these directives are swopped for the properties contained within the mixin they are including.

So, let the preprocessor work on the following SCSS:

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
```

And it would give this CSS:

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
```

Mixins also supports parameters, in the manner of functions. They may have a default value and thus be optional, or they may be passed as lists: 

```scss
@charset 'utf-8';

@mixin round-border($radius: 25%, $numbers...) {
    $result: 1;
    
    @each $number in $numbers {
      $result: $result * $number;
    }
    
    $result: $result+$radius;
    @if $result > 100 {
      $result: 100%;
    }
    
    -webkit-border-radius: $result;
       -moz-border-radius: $result;
            border-radius: $result;
}

.button {
    @include round-border;
}

.some-completely-rounded-div {
    @include round-border(100%, 2, 3, 4);
}
```

## Content ##

Style blocks may also be entirely passed to a mixin.

To support this operation, Sass offers the special ``@content`` directive.

This way, the rules defined within the ``@include`` directive substitutes the mixin's ``@content``:

```scss
@mixin large {
    @media (min-width: 800px) {
        font-size:1.5rem;
        @content;
    }
}

#element {
    font-size:1.2rem;
    
    @include large {
        width:500px;
        height:300px;
    }
}
```

The previous code results in the following:

```css
#element {
  font-size: 1.2rem;
}

@media (min-width: 800px) {
  #element {
    font-size: 1.5rem;
    width: 500px;
    height: 300px;
  }
}
```

## Either/Or ##

If the repeated code must have different values at different times, mixins are the answer. The ``@extend`` directive does not support parameters or default values after all.

In addition, caution must be used when extending selectors, because rules are going to be written right at the beginning of the resulting stylesheet. This might cause later issues, since the succeeding rules may inadvertently overwrite former ones.

However, diffusely applying mixins may as well be not so good for the final result, in that it would repeat styles that could otherwise be joined together in a single-line group of selectors.

Therefore, granted that arguments are not needed and that the rules are not overwritten, the extend technique might be good to diminish code.

In the end, choosing whether to apply mixins or extendings is rather a matter of critical thinking, and this is only possible when the available tools and the expected result are well-known.

## Furthering ##

Practice makes perfect. Study [the official documentation](https://sass-lang.com/documentation) and, after mastering Sass, check some of its derivated frameworks, such as [Compass](http://compass-style.org/).

Moreover, most of the links provided in this introductory tutorial contain in-depth reflections regarding their subjects. Reading those is highly suggested.

Last but not least: take your time. Express yourself. Have fun :)
