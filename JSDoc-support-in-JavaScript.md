The below code outlines which constructs are currently supported or not supported
when using JSDoc annotations to provide type information in JavaScript files.

Note any tags which are not explicitly listed below (such as `@constructor`) are not yet supported.

```js
// === Supported ===

// You can use the "@type" tag and reference a type name (either primitive,
// defined in a TypeScript declaration, or in a JSDoc "@typedef" tag)
// You can use any Typescript type, and most JSDoc types.
/**
 * @type {string}
 */
var var1;

/** @type {Window} */
var var2;

/** @type {PromiseLike<string>} */
var var3;


// Likewise, for the return type of a function
/**
 * @return {PromiseLike<string>}
 */
function fn1(){}

/**
 * @returns {{a: string, b: number}} - May use '@returns' as well as '@return'
 */
function fn2(){}


/**
 * The type specifier can specify a union type - e.g. a string or a boolean
 * @type {(string | boolean)}
 */
var var4;

/**
 * Note that parens are options for union types
 * @type {string | boolean}
 */
var var5;


// You can specify an array type (e.g. an array of numbers)
/** @type {number[]} */
var var6;

// An array of numbers (alternate syntax)
/** @type {Array.<number>} */
var var7;

/** @type {Array<number>} */
var var8;


// An object specification may also be used within the braces
// For example, an object with properties 'a' (string) and 'b' (number)
/** @type {{a: string, b: number}} */
var var9;

// "@typedef" may be used to define complex types
// (this same same syntax works with @param)
/**
 * @typedef {Object} SpecialType - creates a new type named 'SpecialType'
 * @property {string} prop1 - a string property of SpecialType
 * @property {number} prop2 - a number property of SpecialType
 * @property {number=} prop3 - an optional number property of SpecialType
 * @prop {number} [prop4] - an optional number property of SpecialType
 * @prop {number} [prop5=42] - an optional number property of SpecialType with default value
 */
/** @type {SpecialType} */
var specialTypeObject;

// You can use both 'object' and 'Object'
/**
 * @typedef {object} SpecialType1 - creates a new type named 'SpecialType'
 * @property {string} prop1 - a string property of SpecialType
 * @property {number} prop2 - a number property of SpecialType
 * @property {number=} prop3 - an optional number property of SpecialType
 */
/** @type {SpecialType1} */
var specialTypeObject1;


// Parameters may be declared in a variety of syntactic forms
/**
 * @param p0 {string} - A string param declared using TS-style
 * @param {string}  p1 - A string param.
 * @param {string=} p2 - An optional param
 * @param {string} [p3] - Another optional param.
 * @param {string} [p4="test"] - An optional param with a default value
 * @return {string} This is the result
 */
function fn3(p0, p1, p2, p3, p4){
  // TODO
}

// Index signatures
/** @type {Object.<string, object>} */
var map;
/** @type {Object.<number, object>} */
var arrayLike;
// equivalent to { [s: string]: any } and { [n: number]: any }


// Generic types may also be used
/**
 * @template T
 * @param {T} p1 - A generic parameter that flows through to the return type
 * @return {T}
 */
function fn4(p1){}

// Define function type
/** @type {function(string, boolean): number} */
var fn5;

// Both "fn6" and "fn7" have same type of Function type.
/** @type {function} */
var fn6;

/** @type {Function} */
var fn7;

/**
 * @param {*} p1 - Param can be 'any' type
 * @param {?} p2 - Param is of unknown type (same as 'any')
 */
function fn8(p1, p2){}

var someObj = {
  /**
   * @param {string} param1 - Docs on property assignments work
   */
  x: function(param1){}
};

/**
 * As do docs on variable assignments
 * @return {Window}
 */
let someFunc = function(){};

/**
 * And class methods
 * @param {string} greeting The greeting to use
 */
Foo.prototype.sayHi = (greeting) => console.log("Hi!");

/**
 * And arrow functions expressions
 * @param {number} x - A multiplier
 */
let myArrow = x => x * x;

/**
 * Which means it works for stateless function components in JSX too
 * @param {{a: string, b: number}} test - Some param
 */
var sfc = (test) => <div>{test.a.charAt(0)}</div>;

/**
 * A parameter can be a class constructor.
 *
 * @param {{new(...args: any[]): object}} C - The class to register
 */
function registerClass(C) {}

/**
 * A map-like object that maps arbitrary `string` properties to `number`s.
 *
 * @type {Object.<string, number>}
 */
var var10;

/**
 * Listing properties that exist on `param1` through dotted param names.
 *
 * @param {object} param1
 * @param {string} param1.name
 */
function fn8(param1) {
  return param1.name;
}

/**
 * @param {...string} p1 - A 'rest' arg (array) of strings. (treated as 'any')
 */
function fn10(p1){}

/**
 * @param {...string} p1 - A 'rest' arg (array) of strings. (treated as 'any')
 */
function fn9(p1) {
  return p1.join();
}

// We can "cast" types to other types using a JSDoc type assertion
// by adding an `@type` tag around any parenthesized expression.

/**
 * @type {number | string}
 */
var numberOrString = Math.random() < 0.5 ? "hello" : 100;
var typeAssertedNumber = /** @type {number} */ (numberOrString)
```

## Patterns that are known NOT to be supported

```js
function SomeFunction(){}
/**
 * Refering to objects in the value space as types isn't guaranteed to work.
 *
 * @type {SomeFunction}
 */
var var11;

/**
 * Optional members of object literals (optionality is ignored)
 *
 * @type {{a: string, b: number=}}
 */
var var12;

/**
 * A nullable number (only works if `strictNullChecks` is on)
 *
 * @type {?number}
 */
var var13;

/**
 * A 'non-nullable number (treated as just `number`)
 *
 * @type {!number}
 */
var var14;

// Inline JsDoc comments (treated as 'any')
function fn10(/** string */ p1) {}
```
