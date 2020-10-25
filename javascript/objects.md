# Objects

Objects in JavaScript can have a literal form (`var a = {value: 3}`) or a constructed form (`var a = new Array()`).

Objects are of 6 primitive types:

```
string
number
boolean
null
undefined
object
```

Objects are a collection of key/value pairs. The values can be accessed as properties with `.propName` or `["propName"]`.

Properties have characteristics that can be controlled with **property descriptors** such as `writable`.

```javascript
var myObject = {
	a: 2
};

Object.getOwnPropertyDescriptor( myObject, "a" );
// {
//    value: 2,
//    writable: true,
//    enumerable: true,
//    configurable: true
// }
```

Objects can have their mutability modified using `Object.freeze(..)` or `Object.seal(..)`.

Properties don't have to hold a value, they can be **accessors properties** with getters/setters.


Object.assign(..) is an ES6+ utility for doing a shallow assignment copy of properties from one
or more source objects to a single target object: Object.assign( target, source1, ... )

When to use Map:

if you need some complex data types as keys, use Map. Pretty useful for coding algorithm questions. Take a look at this Leetcode question for example.
if you need the insertion-ordering of your keys, use Map.
when in doubt, just use a Map. Map is more flexible in use-cases and has consistent syntax. You just can’t go wrong with it.

When to use Object:

if you need to work with JSON at all, use Object. JSON won’t work with Map. Here’s a Stack Overflow post on that.
If you need to use complex data type keys and JSON, look up how to serialize a Map.
if you know you only need simple data types as keys (String, Integer, Symbol), consider using an Object. Object is probably a little more performant than a Map ({} vs new, direct-access getters and setters vs function calls).

[https://dandkim.com/js-map-vs-object/]