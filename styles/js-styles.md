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

#### Vertical whitespace

#### Horizontal whitespace

#### Horizontal alignment

#### Function arguments

### Comments

#### Block comment style


## Language features

### Local variables

### Array literals

### Object literals

### Enums

### Classes

### Functions

### String literals

#### Template strings and HTML

### Number literals

### This

### Non-standard language features


## Naming

### Common naming rules

### Naming rules by identifier




```javascript
```