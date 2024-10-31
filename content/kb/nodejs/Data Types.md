

JavaScript is a loosely typed dynamic, prototypal language with seven primitive types.

Everything else, including functions and arrays, is an object.

primitives:

- Null: **null**
- Undefined: **undefined**
- Number: **1**, **1.5**, **-1e4**, **NaN**
- BigInt: **1n**, **9007199254740993n**
- String: **'str'**, **"str"**, **`str ${var}`**
- Boolean: **true**, **false**
- Symbol: **Symbol('description')**, **Symbol.for('namespace')**

The **null** primitive is typically used to describe the absence of an object

**undefined** is the absence of a defined value.
- Any variable initialized without a value will be **undefined**.
- Any expression that attempts access of a non-existent property on an object will result in **undefined**.
- A function without a **return** statement will return **undefined**.

Number type is double-precision floating-point format.
- It allows both integers and decimals but has an integer range of -253-1 to 253-1. 
- The BigInt type has no upper/lower limit on integers.

Strings can be created with single or double quotes, or backticks.
- Strings created with backticks are template strings, these can be multiline and support interpolation
- normal strings can only be concatenated together using the plus (+) operator.

Symbols can be used as unique identifier keys in objects.
- **Symbol.for** method creates/gets a global symbol.

An object is a set of key value pairs
- values can be any primitive type or an object
- keys are called properties.
- An object with a key holding a value that is another object allows for nested data structures:

```js
const obj = { myKey: { thisIs: 'a nested object' } }  
console.log(obj.myKey)
```

A prototype is an implicit reference to another object that is queried in property lookups.
- All Javascript objects have prototypes
- If an object doesn't have a particular property, the object's prototype is checked for that property.
- If the object's prototype does not have that property, the object's prototype's prototype is checked and so on. This is how inheritance in JavaScript works