---
layout: page
title: Style Guide - JavaScript
published: true
---


## JavaScript Styles

This page serves as the definition of our JavaScript coding standards. It is a mixture of best practices, our own internal coding habits and preferences, and is built upon [Google's own JS Guide](https://google.github.io/styleguide/jsguide.html).

## Source files and structure


### File names

JS files must be all lowercase and may include underscores (`_`) or dashes (`-`), but no additional punctuation. Ideally, our naming convention should be the same across projects, but consistency per project is preferred over strict convention. In other words, follow the convention that each project uses. 

Filenames’ extension must be .js.

**Good examples**

- project_loader.js
- myfilename.js
- a-long-js-filename-here.js

### File structure

A source file consists of, in order:

- License or copyright information, if present or required
- @fileoverview JSDoc, if present
- Required files or modules (e.g. `require('afile')` or `import Module from "./my-module";`), if necessary
- The file’s implementation
- Export of the module (**N.B.** only needed for package manager environments where this is a module to export)

Exactly one blank line separates each section that is present, except the file's implementation, which should be preceded by 1 or 2 blank lines.

**Good example**

```javascript
/**
* Licence
*
* author: 	author's name
* ... 
* item: 	description
* 
*/

/**
 * @fileoverview Description of file, its uses and information
 * about its dependencies.
 * @package
 */

 // Required modules
 require('jquery')
 require('map-loader')

 OR

 import MyModule from "./my-module";


 // Implementation
 const myModule = (function(){
 	var myVariable = 123
 	..
 	..
 	// rest of implementation 
 	..
 	})();
```

#### License or copyright information

If license or copyright information belongs in a file, it belongs here.

### File implementation

Where possible, each file should be a complete unit of modular functionality, able to be exported and imported as needed. 

Files should follow good coding standards, particularly:

- Be open for extension, but closed for modification
- As [SOLID](https://en.wikipedia.org/wiki/SOLID) as possible
- Efficient, well commented and [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)

Files should be structured in one of the following ways:

- For non-NodeJS files or those that use `module.exports` conventions, the [Revealing Module Pattern](https://addyosmani.com/resources/essentialjsdesignpatterns/book/) is the preferred implementation
- For Node-esque files or those that can be loaded by package managers such as Yarn or Webpack, these can be structured in such a way that they can be exported by said package manager

#### Revealing module pattern

This pattern generates a self-executing anonymous function, stores it in a variable and allows for the private and public exposure of required members, properties and functions.

These modules should start with a capital letter and called as such when required in another file or module.

Each module should return a list of public members named the same as their internal counterpart.

**Good example**

```javascript
const MyModule = (function(){

	// private variables
	let item1 = 123;
	let item2 = 456;

	// private methods
	const myPrivateFunction = function() {
		.. // implementation
	};


	// public methods
	const myPublicFunction = function(arg1, arg2, ..) {
		.. // implementation
	};


	// return object for public access
	return {
		myPublicFunction: myPublicFunction
	};
})();


// This can then be called anywhere else as such

MyModule.myPublicFunction('argument1', 'argument2');
```

#### Node JS style modules

These files can be authored in a very similar way depending on their complexity. For example, a simple module doesn't need to be wrapped in a revealing module enclosure. However, a more complex example should be wrapped in the revealing module pattern and exported as a variable at the end of the document.

- Export name should be the same as the module variable name in the document


**Good examples**

A simple export module

```javascript
const MySimpleModule = {
    variable_option: 'some variable value'
};

export default MySimpleModule;
```

A more complex export module

```javascript
// imports first
import AnotherModule from './another_module';

// implementation
const MyModule = (
    function(w, document){

        let myPublicFunction = function(args1, args2) {
        	..
        	.. // implementation
        	..
        };

        return {
            myPublicFunction: myPublicFunction
        }
    }
)(window, document);

// exports last - same name as the implementation variable above
export default MyModule;
```

## Syntax and formatting

It is important that all our code is consistent and adheres to a common set of formatting guidelines. Keeping things uniform allows for easier reading and digesting, aids maintenance and refactoring, and ultimiately helps up ship better code.

### Braces

Braces are required for all control structures (i.e. `if`, `else`, `for`, `do`, `while`, as well as any others), even if the body contains only a single statement. The first statement of a non-empty block must begin on its own line.

**Exception**: A simple `if` statement that can fit entirely on a single line with no wrapping (and that doesn’t have an else) may be kept on a single line with no braces when it improves readability. This is the only case in which a control structure may omit braces and newlines.

`if (shortCondition()) return;`

**Good example**

```javascript
if(someVeryLongCondition()) {
	.. // statement body
}

for(let i = 0; i < thing.length; i++) {
	.. // statement body
}
```

#### Non-empty blocks

Braces follow the Kernighan and Ritchie style (["Egyptian brackets"](https://blog.codinghorror.com/new-programming-jargon/)) for nonempty blocks and block-like constructs:

- No line break before the opening brace.
- Line break after the opening brace.
- Line break before the closing brace.
- Line break after the closing brace if that brace terminates a statement or the body of a function or class statement, or a class method. Specifically, there is no line break after the brace if it is followed by else, catch, while, or a comma, semicolon, or right-parenthesis.

```javascript
method(foo) {
    if (condition(foo)) {
      try {
        // Note: this might fail.
        something();
      } catch (err) {
        recover();
      }
    }
  }
```

#### Empty blocks

An empty block or block-like construct may be closed immediately after it is opened, with no characters, space, or line break in between (i.e. {}), unless it is a part of a multi-block statement (one that directly contains multiple blocks: if/else or try/catch/finally).

```javascript
function doNothing() {}
```

### Blocks

#### Indentation

Each time a new block or block-like construct is opened, the indent increases by four (4) spaces. When the block ends, the indent returns to the previous indent level. The indent level applies to both code and comments throughout the block

#### Arrays and Objects

Arrays and object literals have a very similar look to them in terms of how they can be declared. We format them the same, in one of two ways, preferring readability first, then conciseness:

1. Inline declaration
2. Block-like declaration

**Good example**

```javascript
// Arrays
const a = [
  0,
  1,
  2,
];

const b = 
    [0, 1, 2];

const c = [0, 1, 2];


// Objects
const a = {
  a: 0,
  b: 1,
};

const b =
    {a: 0, b: 1};
```

#### Function expressions

When declaring an anonymous function in the list of arguments for a function call, the body of the function is indented two spaces more than the preceding indentation depth.

**Good example**

```javascript
some.reallyLongFunctionCall(arg1, arg2, arg3)
    .thatsWrapped()
    .then((result) => {
      // Indent the function body +2 relative to the indentation depth
      // of the '.then()' call.
      if (result) {
        result.use();
      }
    });
```

#### Switch statements

As with any other block, the contents of a switch block are indented +4.

After a switch label, a newline appears, and the indentation level is increased +2, exactly as if a block were being opened. An explicit block may be used if required by lexical scoping. The following switch label returns to the previous indentation level, as if a block had been closed.

A blank line is optional between a break and the following case.

**Good example**

```javascript
switch (animal) {
    case Animal.BANDERSNATCH:
      handleBandersnatch();
      break;

    case Animal.JABBERWOCK:
      handleJabberwock();
      break;

    default:
      throw new Error('Unknown animal');
}
```

### Statements

Everything outside of a block opening or closing line is generally regarded as a statement. These are the individual lines comprising the main functionality, creating/assign variables, calling functions, etc.

These are the basic rules for statements:

- One statement per line; Each statement is followed by a line-break.
- Semicolons are required - Every statement must be terminated with a semicolon. Relying on automatic semicolon insertion is forbidden.
- JavaScript is not limited per line, but readbility should be preferred at all times. A generally agreed upon standard is limiting lines to 80 characters, but using best judgement is allowed. It is accepted that some lines (those containing URL's for example) will not be able to be gracefully split.

### Whitespace

Since our minification and compression tools will strip out any superfluous whitespace and comments, there is no reason to not make liberal use of both of these to inject clarity into our code.

#### Vertical whitespace

Vertical whitespace should be used liberally to separate sections of related code and introduce clarity and readability. 

Here are the blank lines count that you should use and where to use them:

- One (1) empty line:
    - Between consecutive methods in a class or object literal (**Exception:** A blank line between two consecutive properties definitions in an object literal (with no other code between them) is optional. Such blank lines are used as needed to create logical groupings of fields.)
    - At the beginning of a method body, usually where you will begin with variable definition
    - Within method bodies, sparingly to create logical groupings of statements. Blank lines at the end of a function body are not allowed.
- Four (4) empty lines:
    - Between larger, related sections within a class. For example, between a large variable list and private function definition

**Good example**

```javascript
/**
 * Introduction comment about the class or method
 * @param {string} arg1 - this does something
 */

const MyClass = function() {

	// PRIVATE VARIABLES //
	
	var someVar = "something",
		anotherVar = "something else",
		finalVar = 123;




	// PRIVATE METHODS //

	const myPrivateFunc = function(arg1) {

		var someVar = 3;
		
		for(var i = 0; i < someVar; i++) {
			...
		}
		
		return someVar;
	}

	/**
	* This comment introduces the function and it's arguments
	* @param {Array} args - this does something magical
	*/
	const myOtherPrivateFunc = function(args, moreArgs) {

	}
}
```

#### Horizontal whitespace

Use of horizontal whitespace depends on location, and falls into three broad categories: leading (at the start of a line), trailing (at the end of a line), and internal. Leading whitespace (i.e., indentation) is addressed elsewhere. Trailing whitespace is forbidden.

Beyond where required by the language or other style rules, and apart from literals, comments, and JSDoc, a single internal ASCII space also appears in the following places **only**.

- Separating any reserved word (such as `if`, `for`, or `catch`) from an open parenthesis (`(`) that follows it on that line.
- Separating any reserved word (such as `else` or `catch`) from a closing curly brace (`}`) that precedes it on that line.
- Before any open curly brace (`{`), with two exceptions:
    - Before an object literal that is the first argument of a function or the first element in an array literal (e.g. `foo({a: [{c: d}]})`).
    - In a template expansion, as it is forbidden by the language (e.g. `abc${1 + 2}def`).
- On both sides of any binary or ternary operator.
- After a comma (`,`) or semicolon (`;`). Note that spaces are never allowed before these characters.
- After the colon (`:`) in an object literal.
- On both sides of the double slash (`//`) that begins an end-of-line comment. Here, multiple spaces are allowed, but not required.

#### Function arguments

Function arguments should all live on the same line as the function name. However, if in doing so you start to get lots of line-wrapping, it is preferred to put each argument on its own line to enhance readability.

**Good example**

```javascript
// Preferred, common argument definition, all on one line with function name
function myFunction (shortArg1, shortArg2) {
  // …
}

// Alternative argument definition where argument names are long or numerous or both
function myOtherFunction (
	aReallyDescriptiveLongName,
	anotherReallyLongHelpfulArgumentName
	oneLastHugeArgumentNameThatIsDescriptive) {
  // ...
}
```

### Comments

It's difficult to overdo comments as they are supremely important in helping team members to understand what sections of code do, how they function, what arguments they take and the return objects the may provide.

The preferred commenting styles are outlined below, but the main point to remember is make sure that the comments are:

- Descriptive enough to be valuable and useful
- Concise enough to be clear
- Necessary and accurate (e.g. do you really need a comment here or is the code self-evident?)

#### Block comment style

Block comments are indented at the same level as the surrounding code. There are a few different formats which are preferred for different locations within a piece of code, but the common examples are shown below.

**Good examples**

```javascript
/**
 * This is a larger, introduction comment that describes how the complete module works
 * outlines any caveats, use-cases or links to relevant documentation or websites
 * It might also include an author statement or implementation example
 */

const MyClass = function() {

	// PRIVATE VARIABLES //		- This is a section comment that breaks up a discrete block
	
	var someVar = "something",
		anotherVar = "something else",
		finalVar = 123;




	// PRIVATE METHODS //

	const myPrivateFunc = function(arg1) {

		var someVar = 3;
		
		// this is a brief comment that explains this part
		for(var i = 0; i < someVar; i++) {
			...
		}
		
		return someVar;
	}

	/**
	* This comment introduces the function and it's arguments
	* @param {Array} args - this does something magical
	*
	* returns {Object} myObj - a collection of some other magical things
	*/
	const myOtherPrivateFunc = function(args, moreArgs) {

	}
}
```

## Language features

JavaScript includes many dubious (and even dangerous) features. This section delineates which features may or may not be used, and any additional constraints on their use.

### Local variables

Declare all local variables with either `const` or `let`. Use `const` by default, unless a variable needs to be reassigned. 

The `var` keyword should be avoided.

Multiple variables can be definied in one block, provided they don't rely on the definition of other variables in that same block.

**Good example**

```javascript
const myUnchangingVar = "this is a string";

let someVar = 3,
	anotherVar = 4,
	arrVar = [1, 2, 3];
```

#### Variable scope

Local variables are not habitually declared at the start of their containing block or block-like construct. Instead, local variables are declared close to the point they are first used (within reason), to minimize their scope.

The exception to this is for class-wide variables that are global to the class and containing methods. These should be declared in a logical group at the beginning of the file, but initialised as part of an `init()` method as soon as possible.

### Array literals

Arrays should be declared using the shorthand declaration and **not** the Array constructor. They should include a trailing comma whenever there is a line break between the final element and the closing bracket.

**Good example**

```javascript
let values = [1, 2, 3];

let moreValues = [
	'one value',
	'another value',
]; 
```

### Object literals

In a similar vein to Array literals, Objects should be declared using the shorthand notation and include a trailing comma whenever there is a line break between the final property and the closing brace.

**Don't** mix quoted and unquoted keys. The preferred style is to use unquoted keys.

**Good example**

```javascript
let myObj = {
	aKeyName: "some value",
	anotherKey: 23,
	};
```

### Enums

Enums are essentially Object literals, but they are constant, fixed values and are defined using uppercase key names.

They also have a capitalised variable name to define them.

```javascript
const Options = {
	FIRST_OPTION: 1,
	SECOND_OPTION: 2,
	};
```

### Classes

**To be defined**: Class declaration is still in discussion and this section will be updated in the future.

### Functions

On the whole, functions should be concise, prescriptive and adhere as closely as possible to the DRY principle. If your function is growing large or complex you are encouraged to refactor it, thinking about separating it out into smaller functions or less complex code.

#### Arrow functions

Arrow functions provide a concise syntax and fix a number of difficulties with `this`. Prefer arrow functions over the `function` keyword, particularly for nested functions.

Avoid writing const self = this. Arrow functions are particularly useful for callbacks, which sometimes pass unexpected additional arguments.

The right-hand side of the arrow may be a single expression or a block. Parentheses around the arguments are optional if there is only a single non-destructured argument.

#### Arguments

Be sure to include the name and types of any arguments in the accompanying JSDoc-style function declaration comment.

Optional arguments are allowed but generally discouraged unless using a technology such as Webpack with a plugin that makes newer script style abilities backwards compatible with older browsers.

### String literals

String literals should be defined with double quotes (`"`) as often as possible. Where it makes sense to use single quotes (`'`), however, maintain consistency. Strings should not span multiple lines.

#### Template strings and HTML

When it comes to concatination, avoid using multiple joins with the (`+`) operator. Instead, use a template string that can be edited using a search and replace approach.

Again, for older browsers (and IE) make sure your string templating is passed through a shim or module within a build manager that will inject more compatible code.

```javascript

// new String literal templating
const a = 5;
const b = 10;

console.log(`a and b added are ${a + b});


// older, more compatible way
let output = "a and b added are {1}";
console.log(output.replace("{1}", a + b));
```

### For loops

With ES6, the language now has three different kinds of `for` loops. All may be used, though for-of loops should be preferred when possible.

Prefer `for-of` and `Object.keys` over `for-in` when possible.

**Good example**

```javascript
const myArr = [1, 2, 3, 4, 5];

for (let num of myArr) {
  
  console.log(num);
}

// will output 12345 to the console.
```

### Exceptions

Exceptions are an important part of the language and should be used whenever exceptional cases occur. Always throw `Errors` or subclasses of `Error`: never throw string literals or other objects. Always use `new` when constructing an `Error`.

Custom exceptions provide a great way to convey additional error information from functions. They should be defined and used wherever the native `Error` type is insufficient.

Prefer throwing exceptions over ad-hoc error-handling approaches (such as passing an error container reference type, or returning an object with an error property).

### This

Only use `this` in class constructors and methods, or in arrow functions defined within class constructors and methods. Any other uses of `this` must have an explicit `@this` declared in the immediately-enclosing function’s JSDoc.

Never use `this` to refer to the global object, the context of an `eval`, the target of an event, or unnecessarily `call()`ed or `apply()`ed functions.

## Naming

Identifiers use only ASCII letters and digits, and, in a small number of cases noted below, underscores and very rarely (when required by frameworks like Angular) dollar signs.

Give as descriptive a name as possible, within reason. Do not worry about saving horizontal space as it is far more important to make your code immediately understandable by a new reader. Do not use abbreviations that are ambiguous or unfamiliar to readers outside your project, and do not abbreviate by deleting letters within a word.

### Class names and Enum names

Class, interface, record, and typedef names are written in `UpperCamelCase`. Unexported classes are simply locals: they are not marked @private and therefore are not named with a trailing underscore.

Type names are typically nouns or noun phrases. For example, `Request`, `ImmutableList`, or `VisibilityMode`. 

Enums are very similar to classes in that they use `UpperCamelCase`.

### Method names

Method names are written in `lowerCamelCase`. Private methods’ names must end with a trailing underscore.

Method names are typically verbs or verb phrases. For example, `sendMessage` or `stop_`. Getter and setter methods for properties are not always required, but if they are used they should be named `getFoo` (or optionally `isFoo` or `hasFoo` for booleans), or `setFoo(value)` for setters.

### Constant names

Constant names use `CONSTANT_CASE`: all uppercase letters, with words separated by underscores. There is no reason for a constant to be named with a trailing underscore, since private static properties can be replaced by (implicitly private) module locals.

### Private variables

Private, global variables should be prefixed with a leading `_`. 


**Good examples**

```javascript

// local variables

priceCountReader      // No abbreviation.
numErrors             // "num" is a widespread convention.
numDnsConnections     // Most people know what "DNS" stands for.


// private variables

_somePrivateVariable
_anotherPrivateVariable


// constant variables

CONSTANT_VARIABLE
THIS_IS_CONSTANT


// class names

class MyImportantClassName = ...


// method names

function createNewTimetable (classList, timesArr) {
	..
}

```
