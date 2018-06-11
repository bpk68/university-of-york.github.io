---
layout: page
title: Style Guide - CSS
published: true
---


## CSS/Sass Styles

We use Sass as our styling preprocessor of choice although we actively investigate ways to leverage built-in CSS conventions as they become better supported, such as CSS Variables. We're not overly bothered about the output from our Sass as it is never directly edited and heavily minified. 

However, we do adhere to a number of conventions when editing our Sass files which are based on our solid CSS best practices with extra support for [BEM](http://getbem.com/) nesting and functions.

Whilst the vast majority of our styling is written in Sass, there are parts of some of our systems where we use vanilla CSS. Regardless of the base stylesheet, we do strive to keep our Sass and CSS simple, readable and above all _maintainable_.

For reference, we based our CSS style guide on this [great style guide example](https://github.com/chris-pearce/css-guidelines) by Chris Pearce, and the [CSS Guidelines](https://cssguidelin.es/) from Harry Roberts.


## Syntax And Formatting

Starting right at the top, we want

- four (4) space indents, no tabs;
- 80 character wide columns;
- multi-line CSS;
- meaningful use of whitespace.

**Note** some of our build and linting tools should catch and enforce most of these syntax and formatting rules.

### Multiple files and modular structure

Given Sass' nature of bundling multiple files together from multiple different locations, it makes sense to compartmentalise our style files into modular components.

For example, this is roughly how we split our Pattern Library styles

```
¬src
|--/sass
	|--/base
		|-- _core.scss
		|-- _reset.scss
		|-- _type.scss
	|--/components
		|--/list
			|-- _list.scss 
		|--/pagination
			|-- _pagination.scss
	|-- _style.scss
|--/js
|--/img
|-- ...
```

Where possible, try to keep style files as small and modular as needed to aid with maintenance 

### Table of contents

A good table of contents will explain how styles pin together and what each does, how it relates to another. Although it requires some commitment from the team to maintain and keep up to date, it's worth it.

```
/*------------------------------------*\
    #MAIN
\*------------------------------------*/

/**
 * CONTENTS
 *
 * CORE
 * Fonts...................@font-face declarations (currently empty)
 * Variables...............Variable setup: colours, fonts, measurements etc. 
 * Mixins..................Globally available mixins.
 * Functions...............Helper and utility functions.
 * Extends.................[deprecated] we're trying to remove extends as they cause specificity  issues. DO NOT USE.
 * Normalize...............Levelling the playing field.
 * Reset...................Resetting a few default styles.
 * Base....................Base element styles.
 *
 * COMPONENTS
 * Header..................Header styles, containing logo etc.
 * Footer..................Footer styles.
 * Navigation..............Page navigation.
 * Mobile navigation.......Page navigation, but fopr mobiles.
 * Subnavigation...........Subnavigation list.
 * Modal...................Modal window for lightboxes, popups etc.
 * hCard...................Address card using microformats
 *
 * ...
 * ...
 * ...
 *
 * MISCELLANEOUS
 * Visibility..............Stateful styles for showing/hiding elements at certain sizes
 * Overrides...............overriding styles using !important
 * IE7.....................specific IE7 styles
 * IE8.....................specific IE8 styles
 * Print...................print-friendly pages
 *
 **/
 ```

### Titles

Begin every new major section of a CSS project with a title:

```
/*------------------------------------*\
  #TITLE OF SECTION HERE
\*------------------------------------*/
```

The '#' allows us to find this much easier when searching files.

### CSS Ruleset

Here is how our rulesets should look:

```
[selector] {
    [property]: [value];
    [property]: [value];
    [property]: [value];
    [<- Declaration ->]
}
```

Our chosen format for how CSS rulesets should be written

-each selector on its own new line;
-a space before the opening brace ({);
-the opening brace ({) on the same line as the last selector;
-a space after the colon (:);
-each declaration on its own new line;
-each declaration indented by four (4) spaces;
-a trailing semi-colon (;) at the end of all declarations;
-the closing brace (}) on its own new line;
-long, comma-separated property values—such as collections of gradients or shadows—arranged across multiple new lines, making sure all values are indented at the same level as the first;

**Good example**

```css
.first-selector,
.second-selector {
    background-color: $color-brand;
    background-image: linear-gradient($color-white, $color-grey-mercury),
                      linear-gradient($color-black, $color-grey-alabaster);
    box-shadow: 1px 1px 1px $color-black,
                2px 2px 1px 1px $color-grey-mercury inset;
    color: $color-text-base;
    display: block;
    padding: rem($spacing-base);
}
```

Adding to the above, we also want to pay attention to

-use lowercase and preferably the shorthand version for all hexadecimal units although if you wish to use the longhand version then this is fine;
-use shorthands for properties and property values where it makes sense; where it doesn't make sense is using a shorthand property that makes you declare zero-values, here it is better to be explicit even if it means more lines of CSS;
-use single quotes for strings, values, etc; The exception to this is for `url()` values;
-wrap attribute selector values in double quotes;
-include a space after each comma in comma-separated values;
-parentheses should not be padded with spaces;
-when a decimal mark is needed always include the zero;
-use double colons (`::`) for pseudo elements;
-use relative units as much as possible (we have Sass mixins to help with this!) - pretty much everything is rem based and outside of font sizing ems may be used for spacing within a component for scaling based on font size, but this should be checked to see what the current approach is as we want consistency with this;
-use the px unit for fixed-sized things, (although this is rare nowadays as we're building responsive layouts) and for the following CSS properties:
--border-radius
--border
--box-shadow
--text-shadow

**Good example**

```css
.my-selector {
    background-image: url(/path/to/image.png);
    color: #eee;
    color: rgba(0, 0, 0, 0.8);
    font-size: 0.98rem;
    padding: rem(12);
    margin-left: rem(6);
    margin-right: rem(6);

    &::before {
        background-color: $color-black;
        content: 'u\123';
    }

    &[type="text"] {
        …
    }
}
```

#### Shorthand Properties

To expand on this rule: use shorthands for properties and property values where it makes sense, treating your properties this way makes for more maintainable and robust CSS.

A good example of this is using the shorthand background-color property instead of the longhand background property when you only need to declare a colour. Using the shorthand background-color property means we now don't have to worry about potentially having to override all of the properties that come bundled with the longhand background property, which are:

```
background-image: initial;
background-position-x: initial;
background-position-y: initial;
background-size: initial;
background-repeat-x: initial;
background-repeat-y: initial;
background-attachment: initial;
background-origin: initial;
background-clip: initial;
background-color: transparent;
```

#### Multi-line CSS

Our style rules should be written across multiple lines, except in very specific circumstances. There are a number of benefits to this:

-A reduced chance of merge conflicts, because each piece of functionality exists on its own line.
-More ‘truthful’ and reliable diffs, because one line only ever carries one change.

Exceptions to this rule are in places where it improves readability, such as similar rulesets that only carry one declaration each, for example:

```
.icon {
  display: inline-block;
  width:  16px;
  height: 16px;
  background-image: url(/img/sprite.svg);
}

.icon--home     { background-position:   0     0  ; }
.icon--person   { background-position: -16px   0  ; }
.icon--files    { background-position:   0   -16px; }
.icon--settings { background-position: -16px -16px; }
```

These types of ruleset benefit from being single-lined because

-they still conform to the one-reason-to-change-per-line rule;
-they share enough similarities that they don’t need to be read as thoroughly as other rulesets - there is more benefit in being able to scan their selectors, which are of more interest to us in these cases.


### Sass Specifics

Syntax and formatting rules specifically for Sass code

-only use the parent selector reference (&) for these use cases
--appending it to pseudo classes;
--appending it to pseudo elements;
--appending it to State hooks in order to chain it to the selector it's referencing;
--referencing itself when combined with any of the above to avoid duplicating CSS properties;
--referencing itself when using BEM element or modifier selectors;
-variable names should include a '-' between multiple words, e.g. '`$primary-color`' **not** '`$primaryColor`';
-only use silent placeholder selectors with the @extend directive, however, we'd rather avoid @extend as much as possible;
-avoid selector nesting, if you have to nest then limit it to one level deep;
-when adding a unit to a number stored in a setting, you have to multiply the number by 1 unit;
-hexadecimal units should not exist outside of global and local partial settings i.e. all colours need to be stored in a setting using a meaningful name;
-always use colour settings for rgb values;
-include parentheses in mixins, even if the mixin is argument-less;
-all Sass functions and mixins should use the [SassDoc](http://sassdoc.com/) documentation guidelines;
-for conditional statements:
--include parentheses;
--always an empty new line before @if;
--@else statements on the same line as previous closing brace (});
--always an empty new line after the last closing brace (}) unless the next line is a closing brace (});

**Good example**

```sass
.selector {
    @extend %selector;
    @include h-text-truncate();

    // Declarations
    background-color: rgba($color-black, 0.5);
    color: $color-text-base;
    padding: $padding-setting * 1rem;


    // State changes
    &,
    &:hover,
    &:focus {
        color: $color-text-base;
    }

    &:hover,
    &:focus {
        …
    }

    &::before {
        …
    }

    &.is-active {
        …
    }


    // Elements
    &__nested-selector {
    	...
    }


    // Modifiers
    &--modifier {
    	...
    }
}


.parent-selector .selector {
    …
}


@if ($support-legacy || $other-condition) {
    …
} @else {
    …
}

/// Remove a unit from a number.
///
/// @author Chris Pearce
/// @access private
///
/// @param {Number [unit]} $number — Number to remove unit from
/// @returns {Number}
///
/// @todo Add @exception rules, see: https://gist.github.com/terkel/4373420
///
/// @example scss - Usage
///     strip-unit(24px)
///     strip-unit(2.3em)

@function strip-unit($number) {
    @if type-of($number) == "number" and not unitless($number) {
        @return $number / ($number * 0 + 1);
    }
    @return $number;
}
```

### Declaration Order

TBA...alphabetical, grouped by function?

**Good example**

```css
.selector {
    bottom: 0;
    display: inline-block;
    left: 0;
    padding-left: rem(10);
    padding-right: rem(10);
    position: absolute;
    right: 0;
    top: 0;
    z-index: 10;
}
```

### More On Ordering

Because we use Sass we can have more than just declarations between the opening and closing braces ({ }) of a ruleset therefore we want to follow a specific order, which is:

-extend calls (@extend);
-mixin calls (@include) with no @content;
-declarations;
-pseudo classes—combining State hooks here is fine;
-pseudo elements;
-BEM elements (Elements, Modifiers);
-mixin calls (@include) with @content—mainly being media queries;
-nested selectors, limit to one level deep or best to avoid in most cases;

Each section should ideally be labelled with a `//` comment, e.g. '//Declarations' and allow two lines between the next statement block.

**Good example**

```sass
.selector {
    @extend %some-silent-placeholder-selector;
    @include hidpi-bg-img('path/to/image/image.png', 32px);
    

    // Declarations
    bottom: 0;
    display: inline-block;
    left: 0;
    position: absolute;
    right: 0;
    top: 0;
    z-index: 10;


    // State
    &:hover,
    &:focus,
    &.is-active {
        …
    }

    &::before {
        …
    }


    // Media queries
    @media (min-width: bp(lap)) {
        …
    }


    // Elements
    &__element {

    }


    // Modifiers
    &--highlighted {

    }


    // Nested
    .nested-selector {
        …
    }
}
```


### Reasonable Number of Characters Wide

Where possible, limit CSS files' width to a reasonable number. The common theme across the Internet seems to settle on 80 characters. However, use good judgement and keep lines as concise as possible.

Reasons for this include:

-the ability to have multiple files open side by side;
-viewing CSS on sites like GitHub, or in terminal windows;
-providing a comfortable line length for comments.

There will be unavoidable exceptions to this rule—such as URLs, or gradient syntax—which shouldn't be worried about.

### Meaningful Whitespace

As well as indentation, we can provide a lot of information through liberal and judicious use of whitespace between rulesets. We use:

-One (1) empty line between closely related rulesets.
-Two (2) empty lines between loosely related rulesets.
-Five (5) empty lines between entirely new sections.

```
/*------------------------------------*\
  #SECTION 1
\*------------------------------------*/

.block { 

	&__element-one {

	}

	&__element-two {

	}


	&--modifier {

	}
}





/*------------------------------------*\
  #SECTION 2
\*------------------------------------*/

.block { 

	&__element-one {

	}

	&__element-two {

	}


	&--modifier {

	}
}
```

## Commenting

Well commented code is _extremely_ important and we really need to be heavily commenting our CSS. Take time to describe components, how they work, their limitations, and the way they are constructed. We don't want to leave others in the team guessing as to the purpose of uncommon or non-obvious code.

We want to be using a comment style that is simple and consistent.

Starting right at the top, we want:

-to place comments on a new line above their subject;
-to keep line-length to a sensible maximum - again, use your best judgement and maintain consistency;
-to make liberal use of comments to break CSS code into discrete sections;
-to use 'sentence case' comments and consistent text indentation.

### DocBlock-esque

The default style of comment we use is a [DocBlock](https://en.wikipedia.org/wiki/PHPDoc#DocBlock)-esque style comment. This type of comment begins with `/**` and ends with `*/` and has an `*` at the beginning of every line except when referencing code—the absence of the * is so that the code can easily be copied.

**Good example**

```
/**
 * A component for the most common type of search input which has deep rounded
 * corners, shadows, and a background image of a magnifying glass icon
 * positioned to the left or right side. Visit: /subscribers/ to see an
 * example.
 *
 * @markup
   <form action="" class="c-search-input [modifier]">
       <label for="[id.value]" class="c-search-input__label  u-hide-visually">Label text</label>
       <input type="search" id="[value]" class="c-search-input__input">
       <span class="c-search-input__icon"></span>
  </form>
 */
```

We should be commenting each discrete piece of CSS in a partial or module or component where it makes sense to do so. Either to explain the inherently unobvious parts or any quirks and gotchas or special use cases.

Comments should start with a DocBlock-esque style comment. One (1) or two (2) empty lines are always used between each discrete piece and one (1) empty line used between the comment and its subject.

**Good example**

```
 /**
 * Fall-back for non-supporting Flexbox browsers, using the Modernizr
 * feature detection technique via the ':not' CSS selector to get around
 * any lag of the Modernizr JS loading.
 */

html:not(.flexbox) .c-modal-dialog-underlay {
    …
}
```

### Partial Heading

Every partial or module should have its own heading like so:

```
/*------------------------------------*\
  COMPONENTS > #LISTINGS 
\*------------------------------------*/
```

Breaking this down we have:

-uppercase for text;
-a breadcrumb pattern showing where the partial belongs in the CSS architecture, the last part being the same name as the partial filename;
- a hash on the module/component in question;
-one (1) empty line to come after the heading;


### Partial Intro

A partial intro type comment follows a Partial Heading type comment, think of this like a books Prologue/Preface/Introduction.

It should be structured like this:

-A description—be as detailed as you can here;
-Any attention grabbing comments prefix with N.B.;
-@credit: if applicable any URL(s) to credit where any ideas came from;
-One (1) empty line between each of the above sections and two (2) empty lines coming after the intro.

**Good example**

```
/**
 * A generic drop down helper powered by some JavaScript which toggles a
 * class e.g. `is-visible` on the drop down trigger (the button that makes the
 * drop down visible and invisible) and the target (the actual drop down).
 * This class will be used to make the drop down target visible when the
 * trigger is selected. There is also a version for showing the drop down via
 * the `:hover` pseudo class which is turned off for touch devices.
 *
 * N.B. this helper is dependent on the "Align" helper.
 *
 * @credit
 * http://www.stubbornella.org/content/2010/06/25/the-media-object-saves-hundreds-of-lines-of-code
 *
 */
```

### Inline Comments

Sass allows us to use less verbose CSS comments prefixed with two forward slashes.

This type of comment can be used for inline type comments, typically when you want to comment above a declaration within a ruleset or when you want to use a sub type comment under a DocBlock*-esque* type comment.

These comments should always be above their subject with a space coming after the two forward slashes.

**Good example**

```
/**
 * Settings.
 */

// Colours
$c-pagination-background-color: #000;

$c-pagination-foreground-color: #333;

// Padding
$c-pagination-padding-left: $spacing-base;

$c-pagination-padding-right: $spacing-half;


h1 {
  font-size: rem($font-size-heading-1);
  // This is needed to turn off the top margin set in normalize.css
  margin-top: 0;
  // This is needed to fix a stupid bug in IE 9 ಠ╭╮ಠ
  width: 100%;
}
```

### Naming Conventions

We always want to ensure that all of our classes and settings are meaningfully named and adhering to our set conventions and to not worry about the length of our class names as gzip will compress well written code incredibly well.

Starting right at the top, we use

-lowercase;
-BEM-like naming for most classes;
-hyphen-delimited for everything else;
-namespaces for almost everything.

We don't want (i.e. **DO NOT DO**)

-CamelCase;
-underscores (with the exception of being used in BEM Element selectors or very rare hacks in the nameing section below);
-id's, never.


#### BEM-like Naming

We approach most of our UI building using the [BEM methodology](http://getbem.com/naming/) approach to naming and breaking up our UI components.

BEM, meaning Block, Element, Modifier, is a front-end methodology coined by developers working at Yandex. Whilst BEM is a complete methodology, here we are only concerned with its naming convention. You can read more about [BEM](http://getbem.com/naming/) on the Yandex documentation site.

BEM splits components’ classes into three groups:

-Block: The sole root of the component.
-Element: A component part of the Block.
-Modifier: A variant or extension of the Block.

As a quick, non-complete example:

```css
.car {}
.car__engine {}
.car--started {}
```

Elements are delimited with two (2) underscores (__ ) and Modifiers are delimited by two (2) hyphens (--).

Here we can see that .car {} is the Block; it is the sole root of a discrete entity. .car__engine {} is an Element; it is a smaller part of the .car {} Block. Finally, .car--started {} is a Modifier; it is a specific variant of the .car {} Block.

One of the distinguishing aspects of BEM naming and structuring is the flat specificity structure the files create. Whilst Elements are related to Blocks and Blocks can be modified using Modifiers, the naming conventions are structured to avoid nesting, thus reducing specificty problems.

We use nesting within our Sass files for clarity and to highlight Elements' relationships within their Blocks, but note the use of the '&' parent selector to output a flat CSS structure.

**Good example**

```sass
.listing-item {

	// state modifiers first
	&:hover {

	}


	// Element(s), begin with a '__' double underscore and the '&' selector
	// outputs '.listing-item__title {}'
	&__title {

	}

	&__description {

	}


	// Modifier(s)
	&--highlighted {

	}
}
```

A few DOs and DON'Ts

-DO look for patterns in design to reuse elsewhere
-DO look through the documentation
-DON'T use IDs in your CSS
-DON'T change the Object types (namespaced o-)
-DON'T use inline styles

### BEM conventions continued

From the [BEM](http://getbem.com/) website:

> [BEM](http://getbem.com/) — Block Element Modifier is a methodology that helps you to create reusable components and code sharing in front-end development

Whilst the implementation and in's and out's of BEM is a whole website in itself, we use it to structure all of our style rules and html layouts.

For example, given a typical html figure element like this,

```html
<figure class="c-figure">
  <img class="c-figure__image" src="https://unsplash.it/800/400/?image=973">
  <figcaption class="c-figure__caption c-figure__caption--bottom-right"><i class="c-icon c-icon--camera c-figure__caption-icon"></i> Simple text caption</figcaption>
</figure>
```

we would implement a modular `Figure` component in a `_figure.scss` file as follows,

```sass
.c-figure {

	// Elements
	&__image {
		// styles in here
	}

	&__caption {
		// styles in here
	}

	&__caption-icon {
		// styles in here
	}


	// Modifiers
	&--highlighted {
		// modified/highlighted styles in here
	}
}
```

Notice how we make heavy use of the '&' parent selector symbol to avoid lots of duplication in our writing.

You can read more about our modularised code base on our [Pattern Library](https://www.york.ac.uk/pattern-library/css-components/figures.html) website.


#### Hyphen-delimited

We use hyphen-delimited naming for just about everything:

-Settings/variables (`$my-long-variable`)
-Mixins (`my-mixin-name`)
-Functions (`a-function-name`)
-Animation names (`hover-transition`)
-Filenames (`_some-sass-file.scss`)
-State hooks (`.is-hidden`)
-JS hooks (`js-button-click`)

Whilst brevity is favoured, don't shy away from using a longer name if it is more descriptive, but remember to use hyphenated naming. For example, `.short` is concise but not very descriptive, whereas `.short-intro-text` is preferred. 

#### Namespace

Namespaces are a useful way to separate out CSS in to different types. In a large-scale website, it can be hard to know what CSS it's possible to mess with, and what knock-on effects it might have. Namespaces give us that information.

The namespaces we have are:

##### Objects o-

Be careful modifying these: objects can be used in many different contexts in the site, so changing the CSS may change more than your local context. Examples are: the media object, the grid layout

##### Components c-

The bread and butter of our CSS. Most parts of the site are components. A component should be able to live in any context and not change, so updating the CSS for a component should bear in mind that capability. Examples: buttons, icons, pagination.

##### Utilities u-

Utility classes usually have a single piece of functionality. Therefore they shouldn't be altered or amended.

##### Themes t-

This signifies that the styles are to be applied on a themed page. Theme pages might be signifying a different page colour, or a different layout. Theme styles should be cosmetic changes, not structural. Examples: 404 page, dark UI, departmental colours.

##### Scopes s-

Scopes are the only time that you will see HTML elements being directly styled in our CSS. It is for areas of the site where the content is user-managed (such as a rich text area) and will not have classes attached. They should give default styling for generic user input. Examples: CMS rich text editors.

##### States is- and has-

Movable styles that can be applied either when the page is loaded or by JS when the state changes. It shows a temporary, optional or short-lived style. Examples: `.is-open`, `.is-active`.

##### Hacks _

An underscore at the start of a class name shows that we're only putting this class here as a hack, it should be used sparingly, and never extended.

##### Javascript js-

We separate classes used for Javascript hooks from CSS classes. This means they are not bound together. Examples: .c-tabs also has a `.js-tabs` class to allow JS to apply tab behaviour.

#### Settings

##### Global Settings & variables

Global settings and variables - these are defined in the global settings/variables partial (`_variables.scss`) - are namespaced with `g-` ("g" stands for "global") then followed by the name of the group they are a part of as the first word then after that any sub-groups.

**Good example**

```css
$g-color-state-error: #dc322f;

$g-color-state-success: #859900;

$g-color-state-warning: #b58900;

$g-color-state-information: #268bd2;
```

##### Local Settings

Local settings and variables — e.g. those defined in partials outside of the global settings partial (`_settings.scss`) — should start with their relevant namespace, followed by the name of what it belongs too, for example, a component, a helper, etc., followed by what the setting is targeting. The main goal is to make the settings as readable as possible.

/**
 * Settings.
 */

// Colours
$c-drop-down-menu-background-color: $g-color-white;

$c-drop-down-menu-outline-color: rgba($g-color-black, 0.19);

$c-drop-down-menu-link-color: $g-color-grey-rolling-stone;

// Widths and heights
$c-drop-down-menu-width: 194px;

$c-drop-down-menu-width-narrow: 170px;

$c-drop-down-menu-arrow-width: 13;

$c-drop-down-menu-arrow-height: 7;

// Padding
$c-drop-down-menu-padding: 7;

$c-drop-down-menu-link-padding-sides: 15px;

$c-drop-down-menu-link-padding-ends: 8px;

#### Sizes And Sides

Sizing classes are partially used when it comes to media queries and the grid, but it's in development at the moment and this section needs defining and updating.

### Tooling

The tooling section needs to be definied and updated.

Automated tools, such as linting make it easier to apply our guidelines and to prevent lost time picking apart nonconforming code in code reviews.

#### Linting

Out linting methods and configuration needs to be defined and updated here.
