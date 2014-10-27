
# Type definition

With JSOND the string literals "boolean", "string", and "number"  is used as JSON values to define a required type of value.

	value = boolean / string / number

>The literal names MUST be lowercase.  No other literal names are allowed.

# Value definition

JSOND can also be used to define allowed string, number and integer values.

Types are deduced from the value definition.

## String values

A string value is defined using regular expression*.

	value = regular-expression

## Number (and Integer) values

Allowed numbers (or integer) is defined using either the set or interval notation.

### Set notation

It's also possible to use the set notation* to define a set of numbers.

### Interval notation

JSOND uses the interval notation* to define a interval of numbers or integers.

<insert syntax here> $allow multiple intervals

In JSON there is no notion of an Integer. In JSOND numbers are considered to be Integers unless a decimal component is explicitly defined. $be put last in interval notation


# Exact values

Since JSOND is a superset of JSON all JSON values that is not a type or value definition is in JSOND context considered to be a definition of an exact value.


# Array definition

All defined array elements may be included in the array zero to many times. An empty array defines that no elements may be included in the array.


# Value references

§Since the ISO standardisation* any JSON value is considered to be valid JSON*. _Note that this is currently not supported by a lot of lint tools._

A JSOND value definition can be stored in a file and referenced from other JSOND files as if it's content was included in the referencing document.

<syntax>

§If a string literal contains what could be a .jsond file a parser could look for the .jsond file in the path.

TODO: think about recursiveness


A value cannot be `null` unless it is explicitly defined.
A member must exist unless it has been defined as `undefined`.
