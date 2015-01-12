# MeteorHacks Style Guide (for JavaScript)

> This is derived from [Meteor Style Guide](https://github.com/meteor/meteor/wiki/Meteor-Style-Guide). But we've made some changes, so this is not a exact copy.

This is a set of guidelines for writing clear, organized, high-quality code that is consistent with the code in all MeteorHacks project.(private or public)

All code in pull requests should follow these rules.

## Commit messages

The first line of a commit should be 50 characters or less. This is a soft limit, so that git's command line output is more readable. Use this line as a clear summary of the change. Try using imperative language in this line. A good commit message is "Add reactive variable for CPU clock speed". Worse examples include "cpu clock speed" or "Added Meteor.CPU_speed so that we can avoid a setTimeout in the main example."

Subsequent lines (if any) can be fairly verbose and detailed. Use your judgement.

## Pull Requests

Pull Request(PR) Title should less than 80 characters. Description is must for any pull request and it should conatains neccessory details to test the PR locally without looking at the source code.

**For an example:** If you sending a change to the DB schema, include the new schema in the description and show how to apply with the current app.

## Whitespace

* 2 space indents
* spaces, not literal tabs
* no trailing whitespace
* maximum line length 80 (this is a soft limit)

Try to stick to 80 characters per line. Comments should be line-wrapped to under 80 characters. It's OK to occasionally go over 80 characters if it substantially improves later indentation, or if there is no good way to break the line.

Terminal output. Lines should be kept short enough that they won't wrap on an 80-column-wide window under normal circumstances (it's fine if, if the user gives something a really super unusually long name, that wrapping happens). Or, even better, detect the actual width of the terminal and format according to the actual window width.

## Semicolons

Always use semicolons. It's a must.

## Functions

Every function should contains 25 lines or less. If it exceeded 25 lines, then you are doing something wrong or there is another way to structure the code.

## Spaces between tokens

Separate tokens with a single space as in these examples:

`a = b + 1`, not `a=b+1`

`function(a, b, c) { return 1; }`, not `function(a,b,c) {return 1;}`

`for(i = 0; i < 3; i++)`, not `for(i=0;i<3;i++)`

`a(1, 2, 3)`, not `a(1,2,3)`

`{ a: 1, b: 2 }`, not `{a:1,b:2}`

`if(a)`, not `if (a)`

`if(!a)`, not `if (!a)`

`a++`, not `a ++`

`--b`, not `-- b`

When functions or objects fit entirely on a single line, put a space inside the enclosing braces:

`stack.push({ parent: node, red: true })`, not `stack.push({parent:node, red: true})`

`a(function() { return true; })`, not `a(function() {return true;})`

Single-line arrays don't get that space though:

`samples.concat([1, 2, 3])`, not `samples.concat([ 1, 2, 3 ])`

## Comments

Comments only needed when you are doing something special or needs some explanations.
Name functions and variables in meaning manner. So, you don't need to comment.

If you are overriding an existing method or hacking something. You **must** comment.

Always use `//` rather than `/*`. See below:

~~~js
// this is a comment for foobar
// this is a superior implementation
function FooBar {

}
~~~

## Use camelCase for identifiers

Functions should be named like `doAThing`, `not do_a_thing`

Other variable names get the same treatment: `isConnected`, not `is_connected`.

## Naming Files

When naming files we should `_` instead of using camelcase.

`foo_bar.js`, not `fooBar.js`.

## Private identifiers start with underscores

If a method or property name starts with _, it's a private implementation detail and shouldn't be used from outside the module where it is defined. It could disappear or change at any time.

## Brace style

Use this brace style:

~~~
if(a < 0) {
  console.log("a is negative");
  handleNegativeA();
} else if(a > 0) {
  console.log("a is positive");
  handlePositiveA();
} else {
  console.log("a is zero or NaN");
  handleOtherA();
}
~~~

You should always use braces even if the content is just a one line.

~~~
if(!alreadyDone) {
  doSomething();
}
~~~

not

~~~
if(!alreadyDone)
  doSomething();
~~~

Only use ternary operation to write it in a single line.

~~~
var a = (alreadyDone)? getTheValue() : 10;
~~~

## Return Statement

Return statement should witten using a single line. (with less than 80 chars).

~~~
var result =
  isThisCorrect ||
  thisMightBeWrong ||
  someThingElse;

return result;
~~~

not

~~~
return
  isThisCorrect ||
  thisMightBeWrong ||
  someThingElse;
~~~

## Self only needed

We should always try to ignore self. That means less closures and good memory usage. But this is not rule. If we are using self, it's okay to mix both `self` and `this`.

We don't write functions longer than 25 lines, so `this`, `self` is not an issue.

## Class pattern

* do not use closure's inside the constructor
* avoid using self in the constructor
* define all the properties in the constructor with default values. (don't always use `null`)

~~~
function GoodClass() {
  this.title = ""; // not null, but "";
  this.total = 0;
}
~~~

Always define the function name if it's in the prototype

~~~
GoodClass.prototype.coolMethod = function coolMethod() {

}
~~~

not
~~~
GoodClass.prototype.coolMethod = function() {

}
~~~

## Singletons

Use singletons only when necessary. If the project is npm module or node app, never use a singleton as a global object.

* Use an object if it only contains properties
* If it contains a method (or a few) use a class and create an object
* If the singleton exposed as a global variable, then use make the first letter capital.

~~~
var MetaDataImpl = function(name) {
  this.name = name;
}

MetaDataImpl.prototype.getTitle = function() {
  return this.name;
}

MetaData = new MetaDataImpl("arunoda");
~~~

not

~~~
MetaData = {
  name: "arunoda",
  getTitle: function() {
    return this.name;
  }
}
~~~

## Create apps which can be crashed at anytime

Always try to write application to be recovered from a crash without any future action. Don't try to gracefully shutdown apps. Because there is nothing called gracefully shutdown in practice.

## Equality Testing

Always use ===, not ==.

If you want to compare a number to a string version of said number, use x === parseInt(y), x.toString() === y, or x === +y. It is much more clear what is going on. (Note that those alternatives differ in how they handle non-numeric characters or leading zeros in the string. Only the third alternative gets all the edge cases right.)

## Coercion to boolean

If you have a value that could be truthy or falsey, and you want to convert it to true or false, the preferred idiom is !!:

~~~
var getStatus = function () {
  // Return status (a non-empty string) if available, else null
};
~~~

~~~
var isStatusAvailable = function () {
  return !! getStatus();
};
~~~

But if you are using local variables, you don't need to transform.

~~~
var name = getName();
if(name) {
  // do something
}
~~~

not

~~~
var name = getName();
if(!!name) {
  // do something
}
~~~

## || for default values

|| is handy for introducing default values:

this.name = this.name || "Anonymous";

~~~
_.each(this.items || [], function () { ... });
~~~

> But be careful. This doesn't work if false is a valid value.

## && for conditional function calls

&& can be handy when conditionally calling a function for its value:

~~~
// Preferred
return event && handleEvent(event);

// Not preferred
return event ? handleEvent(event) : null;
We also like to use it to call a function only if some other value is present:

// Preferred
for(var node = first; node; node = node.parent) {
  node.label && node.callback && node.callback(node.label);
}

// Not preferred (unless it's clearer in a particular case)
for(var node = first; node; node = node.parent) {
  if(node.label && node.callback)
    node.callback(node.label);
}
~~~

It's true that this confuses people who haven't seen && used in this way before. But once you're used to it it's easy to read and convenient. We think it's worth the tradeoff.

~~~
a && b || c
~~~

This is an idiom borrowed from Python. It does the same thing as a ? b : c, except that if a and b are both false, it returns c. You should use a && b || c only where a ? b : c won't do the job.

// If we have a name, return the entry for it in Template, if any. Otherwise return {}.
return name && Template[name] || {};

// This is subtly different. It will return undefined if name is truthy but isn't present in Template.
return name ? Template[name] : {};
Take a moment to convince yourself that a && b || c works as advertised, then memorize its behavior and don't give it another thought.
