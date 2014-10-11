# JSOND

## Introduction

JSOND, _may also be written as_ jsond, is a way of defining JSON using JSON. The structure being defined is created using JSON where values are either a nested object, an array, or a string defining the type of data.

### Types

Based on the valid values JSOND simply defines the following types:

- `"string"`
- `"number"`
- `"boolean"`

JSOND has the characteristic that it has the exact same structure as the object being defined. The difference between an JSON example and valid JSOND is that example values has been replaced with one of the defined types.

### Property definitions

Properties can be defined as optional or allowing the value null. Property definitions appear after the property name definition separated with a colon. There are two valid property definitions:

- `undefined`
- `null`

It is also possible use both undefined and null to define the same property by separating the definitions with the pipe character. When no property definition is supplied the property cannot be neither undefined or null.

## Value definitions

In addition to define types, JSOND can also be used to define valid strings, numbers, and content of arrays. Value definitions appear after the type definition separated with a colon. When no value definition is supplied any value of the correct type is considered as valid value.


### Arrays

The type of values inside an array can be defined. E.g. ["string"] defines that the array should only consist of strings.
Arrays can be defined using multiple "types" which means that 0 or more of those types are allowed.
If nothing is defined inside the array it can contain anything. (same for object).
JSOND does not care about duplicate objects or how many items are in the array.
