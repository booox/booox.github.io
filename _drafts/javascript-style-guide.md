List of ContainS

* Whitespace
* Commas
* Semicolons
* Type Casting & Coercion
* Comments
* Naming Conventions
* Variables
* Hosting
* Conditional Expressions & Equality
* Types
* Strings
* Arrays
* Blocks
* Objects
* Functions
* Properties
* Accessors
* Constructors
* Events
* Modules
* License

### Whitespace 空格

* tab 键设为 2 个空格

/// 不好
```js
function() {
∙∙∙∙var name;
}

// 不好
function() {
∙var name;
}

// 好
function() {
∙∙var name;
}
```

* 在左大括号的左边留一个空格

```js
// 不好
function test(){
  console.log('test');
}

// 好
function test() {
  console.log('test');
}

// 不好
dog.set('attr',{
  age: '1 year',
  breed: 'Bernese Mountain Dog',
});

// 好
dog.set('attr', {
  age: '1 year',
  breed: 'Bernese Mountain Dog',
});
```

* 在操作符两边加上空格
```js
// 不好
var x=y+5;

// 好
var x = y + 5;
```

* 在文件的末尾加上一个空行
```js
// 不好
(function (global)
 {
  // ...stuff...
})(this);

// 好
(function (global)
 {
  // ...stuff...
})(this);
```

* 在一个长的方法链中使用缩进来清晰结构
```js
// 不好
$('#items').find('.selected').highlight().end().find('.open').updateCount();

// 好
$('#items')
  .find('.selected')
    .highlight()
    .end()
  .find('.open')
    .updateCount();

// 不好
var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
    .attr('width', (radius + margin) * 2).append('svg:g')
    .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
    .call(tron.led);

// 好
var leds = stage.selectAll('.led')
    .data(data)
  .enter().append('svg:svg')
    .class('led', true)
    .attr('width', (radius + margin) * 2)
  .append('svg:g')
    .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
    .call(tron.led);
```

### Commas 逗号

* 不要让逗号出现在行首

```js
// 不好
var once
  , upon
  , aTime;

// 好
var once,
  upon,
  aTime;

/// 不好
var hero = {
    firstName: 'Bob'
  , lastName: 'Parr'
  , heroName: 'Mr. Incredible'
  , superPower: 'strength'
};

// 好
var hero = {
  firstName: 'Bob',
  lastName: 'Parr',
  heroName: 'Mr. Incredible',
  superPower: 'Strength',
};
```
* 不要出现多余的逗号

### Semicolons 分号

```js
// 不好
(function () {
  var name = 'Skywalker'
  return name
})()

// 好
(function(){
  var name = 'Skywalker';
  return name;
})();

// 好
;(function(){
  const name = 'Skywalker';
  return name;
})();
```


### Type Casting & Coercion

Perform type coercion Perform type coercion at the beginning of the statement at the beginning of the statement.

Strings:

// => this.reviewScore = 9;

// 不好
var totalScore = this.reviewScore + '';
// 好
var totalScore= '' + this.reviewScore;

// 不好
var totalScore = '' + this.reviewScore + 'total score';

// 好
var totalScore = this.reviewScore + 'totalScore';
Use parseInt for Numbers and always with a radix for type casting.

var inputValue = '4';

// 不好
var val = new Number(inputValue);

// 不好
var val = +inputValue;

// 不好
var val = inputValue >> 0;

// 不好
var val = parseInt(inputValue);

// 好
var val = Number(inputValue);

// 好
var val = parseInt(inputValue, 10);
If for whatever reason you are doing something wild and parseInt is your bottleneck and need to use Bits for performance reasons, leave a comment explaining why and what you're doing.

Note: Be careful when using bitshift operations. Numbers are represented as 64-bit value, but Bitshift operations always return a 32-bit integer (source) Bitshift can lead to unexpected behavior for integer values larger than 32 bits. Discussion

// 好
/**
 * parseInt was the reason my code was slow.
 * Bitshifting the String to coerce it to a
 * Number made it a lot faster.
 */
var val = inputValue >> 0;
Booleans:

var age = 0;

// 不好
var hasAge = new Boolean(age);

// 好
var hasAge = Boolean(age);

// 好
var hasAge = !!age;
Comments

Use // for single line comments. Place single line comments on a newline above the subjects of the comment. Put an empty line before the comment

   // 不好
   var active = true; // is current tab
   //good
   // is current tab
   var active = true;

  //bad
  function getType() {
   console.log('fetching type...');
   //set the default type to 'no type'
   var type = this._type || 'no type';
   return type;
  }


  // 好
  function getType() {
    console.log('fetching type...');

  //set the default type to 'no type'
   var type = this._type || 'no type';
   return type;
 }
Use // FIXME: to annotate problems

function Calculator () {
  // FIXME: shouldn't use a global here
   total = 0;

   return this;
}
Use // TODO: to annotate solutions to problems

function Calculator () {

  // TODO: total should be configurable by an options param
  this.total = 0;

  return this;
}
Use /** ... */ for multi-line comments. Include a description, specify types and values for all parameters and return values

// 不好
// make() returns a new element
// based on the passed in tag name
//
// @param <String> tag
// @return <Element> element
function make(tag) {

  // ...stuff...

  return element;
}

// 好
/**
 * make() returns a new element
 * based on the passed in tag name
 *
 * @param <String> tag
 * @return <Element> element
 */
function make(tag) {

  // ...stuff...

  return element;
}
Naming Conventions

Avoid single letter names. Be descriptive with your naming.

// 不好
function q() {
  // ...stuff...
}

// 好
function query() {
  // ..stuff..
}
Use camelCase when naming objects, functions, and instances

// 不好
var OBJEcttsssss = {};
var this_is_my_object = {};
function c() {}
var u = new user({
 name: 'Bob Parr'
});
// 好
var thisIsMyObject = {};
function thisIsMyFunction(){};
var user = new User({
 name: 'Bob Parr'
});
Use PascalCase when naming constructors or classes

// 不好
function user(options) {
  this.name = options.name;
}

var bad = new user({
  name: 'nope',
});

// 好
function User(options) {
  this.name = options.name;
}

var good = new User({
  name: 'yup',
});
Use a leading underscore _ when naming private properties

// 不好
this.__firstName__ = 'Panda';
this.firstName_ = 'Panda';

// 好
this._firstName = 'Panda';
When saving a reference to this use_this.

// 不好
function () {
  var self = this;
  return function () {
    console.log(self);
  };
}

// 不好
function() {
  var that = this;
  return function () {
    console.log(that);
  };
}

// 好
function() {
var_this = this;
  return function() {
    console.log(this);
  };
}
Name your functions. This is helpful for stack traces.

//bad
var log = function(msg){
  console.log(msg);
 };

 //good
 var log = function log(msg){
   console.log(msg);
 };
Variables

Always use var to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. Captain Planet warned us of that.

// 不好
superPower = new SuperPower();

// 好
var superPower = new SuperPower();
Use one var declaration for multiple variables and declare each variable on a newline.

// 不好
var items = getItems(),
var goSportsTeam = true,
var dragonball = 'z';

// 好
var items = getItems();
   goSportsTeam = true,
   dragonball = 'z';
Declare unassigned variable last. This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

// 不好
var i, len, dragonball,
    items = getItems(),
    goSportsTeam = true;

// 不好
var i, items = getItems();
  dragonball;
  goSportsTeam = true;
  len;

// 好
var items = getItems(),
  goSportsTeam = true;
  dragonball,
  length,
  i;
Assign variables at the top of their scope. This helps avoid issues with variable declaration and assignment hoisting related issues.

//bad
function(){
  test();
  console.log('doing stuff..');

  //..other stuff..

  var name = getname();

  if(name === 'test'){
    return false;
  }

  return name;
}

//good
function(){
  var name = getName();

  test();
  console.log('doing stuff..');

  //..other stuff..

  if(name === 'test'){
    return false;
  }

  return name;
}
Assign variables at the top of their scope. This helps avoid issues with variable declaration and assignment hoisting related issues.

//bad
function(){
  var name = getName();

  if(!arguments.length){
    return false;
  }

  return true;
}

//good
function(){
  if(!arguments.length){
    return false;
  }

  var name = getName();

  return true;
}
Hoisting

Variable declarations get hoisted to the top of their scope, their assignment does not

// we know this wouldn't work (assuming there is no notDefined global variable)
function example() {
  console.log(notDefined); // => throws a ReferenceError
}
// creating a variable declaration after you reference the variable will work due to
// variable hoisting. Note: the assignment value of `true` is not hoisted.
function example() {
  console.log(declaredButNotAssigned); // => undefined
  var declaredButNotAssigned = true;
}

// the interpreter is hoisting the variable declaration to the top of the scope.
// which means our example could be rewritten as:
function example() {
  var declaredButNotAssigned;
  console.log(declaredButNotAssigned); // => undefined
  declaredButNotAssigned = true;
}

// using const and let
function example() {
  console.log(declaredButNotAssigned); // => undefined
  declaredButNotAssigned = true;
}
Anonymous function expressions hoist their variable name, but not the function assignment.

function example() {
  console.log(anonymous); // => undefined

  anonymous(); // => TypeError anonymous is not a function

  var anonymous = function () {
    console.log('anonymous function expression');
  };
}
Named function expressions hoist the variable name, not the function name or the function body.

function example() {
  console.log(named); // => undefined
  named(); // => TypeError named is not a function
  superPower(); // => ReferenceError superPower is not defined
  var named = function superPower() {
    console.log('Flying');
  };
}

// the same is true when the function name is the same as the variable name.
function example() {
  console.log(named); // => undefined
  named(); // => TypeError named is not a function
  var named = function named() {
    console.log('named');
  }
}
Function declarations hoist their name and the function body.

function example() {
  superPower(); // => Flying

  function superPower() {
    console.log('Flying');
  }
}
Conditional Expressions & Equality

Function declarations hoist theri name and the function body.

Use === and !== over == and !=.
Conditional expressions are evaluated using coercion with the ToBoolean method and always follow these simple rules:
Objects evaluate to true
Undefined evaluates to false
Null evaluates to false
Booleans evaluate to the value of the boolean
Numbers evaluate to false if +0, -0, or NaN, otherwise true
Strings evaluate to false if an empty string '', otherwise true
if ([0] && []) {
  // true
  // an array (even an empty one) is an object, objects will evaluate to true
}
Use shortcuts.

// 不好
if (name !== '') {
  // ...stuff...
}

// 好
if (name) {
  // ...stuff...
}

// 不好
if (collection.length > 0) {
  // ...stuff...
}

// 好
if (collection.length) {
  // ...stuff...
}
Types

Primitives: When you access a primitive type you work directly on its value

string
number
boolean
null
undefined
var foo = 1;
  bar = foo;

bar = 9;

console.log(foo, bar); // => 1, 9
Complex: When you access a complex type you work directly on its value

object
array
function
var foo = [1, 2];
  bar = foo;

bar[0] = 9;

console.log(foo[0], bar[0]); // => 9, 9
Strings

Use single quotes '' for strings

// 不好
var name = "Bob Parr";

// 好
var name = 'Bob Parr';

// 不好
var fullName = "Bob" + this.lastName;

// 好
var fullName = 'Bob' + this.lastName;
Strings longer than 80 characters should be written across multiple lines using string concatenation.

// 不好
var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

// 不好
var errorMessage = 'This is a super long error that\
 was thrown because of Batman.\
 When you stop to think about \
 how Batman had anything to do \
with this, you would get nowhere \
fast.';

// 好
var errorMessage = 'This is a super long error that' +
 'was thrown because of Batman.' +
  'When you stop to think about' +
  'how Batman had anything to do' +
  'with this, you would get nowhere' +
  'fast.';
When programmatically building up a string, use Array#join instead of string concatenation. Mostly for IE: jsPerf.

var items;
  messages;
  length;
  i;

messages = [{
  state: 'success',
  message: 'This one worked.'
}, {
  state: 'success',
  message: 'This one worked as well.'
}, {
  state: 'error',
  message: 'This one did not work.'
}];

length = messages.length;

// 不好
function inbox(messages) {
  items = '<ul>';

  for (i = 0; i < length; i++) {
    items += '<li>' + messages[i].message + '</li>';
  }

  return items + '</ul>';
}

// 好
function inbox(messages) {
  items = [];

  for (i = 0; i < length; i++) {
    items[i] = messages[i].message;
  }

  return '<ul><li>' + items.join('<li></li>') + '</li></ul>';
}
Arrays

Use the literal syntax for array creation.

// 不好
var items = new Array();

// 好
var items = [];
If you don't know array length use Array#push.

var someStack = [];

// 不好
someStack[someStack.length] = 'abracadabra';

// 好
someStack.push('abracadabra');
When you need to copy an array use Array#slice. jsPerf

var len = items.length;
  itemsCopy = [];
  i;

// 不好
for (i = 0; i < len; i++) {
  itemsCopy[i] = items[i];
}

// 好
itemsCopy = items.slice();
If you don't know array length use Array#push.

function trigger() {
  var args = Array.prototype.slice.call(arguments);
  ...
}
Blocks

Use braces with all multi-line blocks.

// 不好
if (test)
  return false;

// 好
if (test) return false;

// 好
if (test) {
  return false;
}

// 不好
function() { return false; }

// 好
function() {
  return false;
}
Objects

Use the literal syntax for object creation.

// 不好
var item = new Object();

// 好
var item = {};
Don't use reserved words as keys. It won't work in IE8.

// 不好
var superman = {
  default: { clark: 'kent' },
  private: true
};

// 好
var superman = {
  defaults: { clark: 'kent' },
  hidden: true
};
Use readable synonyms in place of reserved words

// 不好
var superman = {
  class: 'alien'
};

// 不好
var superman = {
  klass: 'alien'
};

// 好
var superman = {
  type: 'alien'
};
Functions

Function expressions:

// anonymous function expression
var anonymous = function() {
  return true;
};

// named function expression
var named = function named() {
  return true;
};

// immediately-invoked function expression (IIFE)
(function() {
  console.log('Welcome to the Internet. Please follow me.');
})();
Never declare a function in a non-function block (if, while, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.

Note: ECMA-262 defines a block as a list of statements. A function declaration is not a statement. Read ECMA-262's note on this issue.

// 不好
if (currentUser) {
  function test() {
    console.log('Nope.');
  }
}

// 好
var test;
if (currentUser) {
  test = function test() {
    console.log('Yup.');
  };
}
Never declare a function in a non-function block(if, while, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.

// 不好
function nope(name, options, arguments) {
  // ...stuff...
}

// 好
function yup(name, options, args) {
  // ...stuff...
}
Properties

Use dot notation when accessing properties.

var luke = {
  jedi: true,
  age: 28
};

// 不好
var isJedi = luke['jedi'];

// 好
var isJedi = luke.jedi;
Use subscript notation [] when accessing properties with a variable.

var luke = {
  jedi: true,
  age: 28
};

function getProp(prop) {
  return luke[prop];
}

var isJedi = getProp('jedi');
Accessors

Accessor functions for properties are not required

If you do make accessor functions use getVal() and setVal('hello')

// 不好
dragon.age();

// 好
dragon.getAge();

// 不好
dragon.age(25);

// 好
dragon.setAge(25);
If the property is a boolean, use isVal() or hasVal()

// 不好
if (!dragon.age()) {
  return false;
}

// 好
if (!dragon.hasAge()) {
  return false;
}
It's okay to create get() and set() functions, but be consistent.

function Jedi(options) {
  options || (options = {});
  var lightsaber = options.lightsaber || 'blue';
  this.set('lightsaber', lightsaber);
}

Jedi.prototype.set = function(key, val) {
  this[key] = val;
};

Jedi.prototype.get = function(key) {
  return this[key];
};
Constructors

Assign methods to the prototype object, instead of overwriting the prototype with a new object. Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!

function Jedi() {
  console.log('new jedi');
}

// 不好
Jedi.prototype = {
  fight: function fight() {
    console.log('fighting');
  },

  block: function block() {
    console.log('blocking');
  }
};

// 好
Jedi.prototype.fight = function fight() {
  console.log('fighting');
};

Jedi.prototype.block = function block() {
  console.log('blocking');
};
Methods can return this to help with method chaining.

// 不好
Jedi.prototype.jump = function() {
  this.jumping = true;
  return true;
};

Jedi.prototype.setHeight = function(height) {
  this.height = height;
};

var luke = new Jedi();
luke.jump(); // => true
luke.setHeight(20); // => undefined

// 好
Jedi.prototype.jump = function() {
  this.jumping = true;
  return this;
};

Jedi.prototype.setHeight = function(height) {
  this.height = height;
  return this;
};

var luke = new Jedi();

luke.jump()
  .setHeight(20);
It's okay to write a custom toString() method, just make sure it works successfully and causes no side effects.

function Jedi(options) {
  options || (options = {});
  this.name = options.name || 'no name';
}

Jedi.prototype.getName = function getName() {
  return this.name;
};

Jedi.prototype.toString = function toString() {
  return 'Jedi - ' + this.getName();
};
Events

When attaching data payloads to events (whether DOM events or something more proprietary like Backbone events), pass a hash instead of a raw value. This allows a subsequent contributor to add more data to the event payload without finding and updating every handler for the event. For example, instead of:

// 不好
$(this).trigger('listingUpdated', listing.id);

...

$(this).on('listingUpdated', function(e, listingId) {
  // do something with listingId
});


// 好
$(this).trigger('listingUpdated', { listingId : listing.id });

...

$(this).on('listingUpdated', function(e, data) {
  // do something with data.listingId
});
Modules

The module should start with a !. This ensures that if a malformed module forgets to include a final semicolon there aren't errors in production when the scripts get concatenated.

The file should be named with camelCase, live in a folder with the same name, and match the name of the single export. Add a method called noConflict() that sets the exported module to the previous version and returns this one. Always declare 'use strict'; at the top of the module.

// fancyInput/fancyInput.js

!function(global) {
  'use strict';

  var previousFancyInput = global.FancyInput;

  function FancyInput(options) {
    this.options = options || {};
  }

  FancyInput.noConflict = function noConflict() {
    global.FancyInput = previousFancyInput;
    return FancyInput;
  };

  global.FancyInput = FancyInput;
}(this);
Licence

(The MIT License)

Copyright (c) 2014 Airbnb

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
