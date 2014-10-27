# Introduction

JSOND is a natural way to define JSON.

The purpose of JSOND is to facilitate documentation.

JSOND is designed to be a minimal superset of JSON.

XXX:

JSON Definition (JSOND) is a way of defining JSON using JSON.

It is a simple, yet powerful, definition language conceived to facilitate discussions, replace the use of examples, and enable model generation and validation of JSON structures.

This describes the full JSOND language. It has been designed to be a simple and practical way to define JSON on a whiteboard or in documentation.

In addition to JSON grammar JSOND grammar defines an optional syntax to define a value without explicitly specifying the value.

JSOND, is a way of defining JSON using JSON. The structure being defined is created using JSON where values are either a nested object, an array, or a string defining the type of data.

It is derived from JSON [RFC4627] and Javascript [ECMA].

JSON Definition (JSOND) is a language for defining a JSON structure using a JSON structure formatted according to a few simple rules described in this document.

JSOND is a superset of JSON [RFC4627]. The JSON structure being defined is itself a JSON structure where values are either structured types (objects and arrays), or one of
"string", "number", "boolean".

JSOND was created to support the process of defining JSON structures and replace the use of JSON examples in API documentation, but can also be used to validate existing JSON and generate models in any programming language that support the corresponding JSON data types.

JSOND's design goals were for the definitions not only to be valid JSON, but also have the same structure as the JSON structure being defined.

:XXX

## Conventions Used in This Document

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC2119].

The grammatical rules in this document are to be interpreted as described in [RFC5234].

# JSOND Grammar

JSOND text is JSON text that includes JSOND grammar. JSOND grammar is a superset of JSON grammar [RFC7159]. The rest of this document describes the JSOND grammar.

## Values

A JSOND value MUST be an object, array or a string literal.

	value = object / array / string-literal

## Objects

A JSOND object MUST define all members in the corresponding JSON object. A JSON object MUST NOT contain a member that has not been defined in the JSOND object.

## Arrays

A JSOND array MUST define a list of JSOND values in such way that all corresponding JSON array values are defined by at least one of the values in the JSOND array.

§Arrays can be defined using multiple "types" which means that 0 or more of those types are allowed.
§JSOND does not care about duplicate objects or how many items are in the array.

## Booleans

A JSOND `"boolean"` defines that the JSON value MUST be either true or false.

	string-literal = boolean
	boolean = %x62.6f.6f.6c.65.61.6e	 ; boolean

## Strings

A JSOND `"string"` defines that the JSON value MUST be a string.

	string-literal = string
	string = %x73.74.72.69.6e.67	     ; string

Regular expressions [REGEXP] MAY be used to define a set of strings.

	string-literal = regular-expression

## Numbers and Integers

A JSOND `"number"` defines that the JSON value MUST be a number.

A JSOND `"integer"` defines that the JSON value MUST be a integer.

	string-literal = number / integer
	number = %x6e.75.6d.62.65.72     ; number
	integer = %x69.6e.74.65.67.65.72	 ; integer

Mathematical sets and/or intervals [ISO-80000-2] MAY be used to define a set of numbers.

	string-literal = *( set / interval )

	set = begin-object [ number-literal *( value-separator number-literal ) ] end-object

	interval = begin-array / begin-parenthesis ( ( number-literal value-separator ) / ( number-literal value-separator number-literal ) / ( value-separator number-literal ) ) end-array / end-parenthesis

	begin-parenthesis = %x28	 ; (
	end-parenthesis = %x29   ; )

	number-literal = integer-literal [ frac ] [ exp ]
	integer-literal = [minus] zero / ( digit1-9 *DIGIT )

An interval that is declared using integers defines the corresponding set of integers. An interval MUST be declared using at least one number with an explicit decimal component to define an interval of real numbers.

In an interval the left or right element is OPTIONAL. An undefined left element defines negative infinity. An undefined right element defines positive infinity.

§Infinity symbols SHOULD always accompanied by round brackets.
§if integers are used (no decimals), then only integers are considered

§If no number is specified with a decimal component an integer SHOULD be assumed. If only integers is used to define an interval, only the list of integers SHOULD be be assumed.

§JSOND automatically assumes all numbers to be integers.
 If any number is allowed numbers must/should be written with a decimal component.

§A decimal mark is a symbol used to separate the integer part from the fractional
 part of a number written in decimal form.


The right element SHOULD be greater than the left element. Insignificant whitespace is allowed.

## Optionals

A member can be defined as optional by appending the optional-character to the end of the JSOND name.

	name = name [ optional-character ]
	optional-character = %x3f ; ?

An optional member MAY have the value null. An optional member MAY be undefined in JSON text.

### References

A JSOND value MAY be stored in a file. A file MUST be referenced using the file or http(s) protocol [RFC3986].

	string-literal = [ protocol ] file

The protocol is OPTIONAL if the value can be evaluated as a relative path to a JSOND file.

### Constants

A JSOND string literal value that is not valid JSOND grammar SHOULD be interpreted as a value constant and is thus REQUIRED in JSON text.

XXX: A JSOND value definition can be stored in a file and referenced from other JSOND files as if it's content was included in the referencing document.

XXX: It should also be noticed that while not being a JSOND value null, true, false and numbers are interpreted by JSOND validators.

XXX: Most string literals are valid regular expressions.

XXX: This might be hard to actually have working since almost every expression is a regular expression. For normal things that is the same thing. For other strings that contain regular expression special things, this will be the most common error.
XXX: It actually works for null, true, false and such things

## References

### Normative References

- [RFC7159]  Bray, T., "The JavaScript Object Notation (JSON) Data Interchange Format", RFC 7159, March 2014.
- [RFC5234]  Crocker, D. and P. Overell, "Augmented BNF for Syntax Specifications: ABNF", STD 68, RFC 5234, January 2008.
- [RFC3986]  Berners-Lee, T., Fielding R., and Masinter, L., "Uniform Resource Identifier (URI): Generic Syntax", RFC 3986, January 2005.
- [RFC2119]  Bradner, S., "Key words for use in RFCs to Indicate Requirement Levels", BCP 14, RFC 2119, March 1997.

### Informative References

- [ECMA-404]  Ecma International, "The JSON Data Interchange Format", Standard ECMA-404, October 2013, <http://www.ecma-international.org/publications/standards/Ecma-404.htm>.
- [RFC4627]  Crockford, D., "The application/json Media Type for JavaScript Object Notation (JSON)", RFC 4627, July 2006.
- [ISO-80000-2] http://www.iso.org/iso/home/store/catalogue_tc/catalogue_tc_browse.htm?commid=46202
- [REGEX] http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap09.html
§IEEE.1003-2.1992 there is no good source to say this is the standard for regex BREX PCEX, ...
§Regular expressions SHOULD follow the regular expression
   specification from ECMA 262/Perl 5
## Appendix: Examples

When defining JSON focus SHOULD be on defining the objects and members and it is RECOMMENDED to omit the details. The example below defines a list of products.

```
[
	{
		"id": "integer",
		"name": "string",
		"category": "string",
		"price": "number",
		"reduced": "boolean",
		"url": "string"
	}
]
```

When the time is right JSOND allows for more details to be defined.

```
[
	{
		"id": "[0,)",
		"name": "[a-z0-9]",
		"category": "^(category1|category2)$",
		"price": "(0.0,)",
		"reduced?": "boolean",
		"url": "http://jsond.org/url.jsond"
	}
]
```

In the example above the id MUST NOT be a negative integer, the name MUST only consist of letters a-z and digits 0-9, the category MUST be category1 or category2, the price MUST be any positive number greater than zero, the reduced member may be true, false, null or undefined in json text, the url is defined by the JSOND value in url.jsond, which requires that the url starts with http.



url -> date
name -> slug
category -> set
constant

[1,5]{7,9}


{
  "percent":"[0,100]"
  "percent":"[0.0,1.0]"
  "binary":"[0,1]"
}


An example of corresponding JSON.

```
[
	{
		"id": 1,
		"name": "product1",
		"category": "categoryOne"
		"price": "1.25",
		"onsale": "true",
		"url": "www.example.com/product1"
	},
	{
		"id": 2,
		"name": "product2",
		"category": "category2"
		"price": "5.99",
		"onsale": "false",
		"url": "www.example.com/product2"
	}
]
```

Sending JSOND to a system as a way to search.
JSOND for Configuration for a system of some kind.

# Notes:

- Plural vs singular (try to use singular)
- Which terms should be used and avoided? +declared, ~~represents~~, +element, +member, ~~key/value~~, +name/value, ~~structure~~, +corresponding, ~~value types, value definitions~~, allowed(1), +undefined, +null … (go through these)
- Types are inferred
- Discuss circular dependencies with references
- Is it possible to write JSOND in JSOND? E.g. to define the type of JSOND that can be sent to do a search.
- In JSOND numbers are considered to be Integers unless a decimal component is explicitly defined.
- It is possible to further define the real number using a set [80000-2:5:2-5.3] of real numbers, or interval [80000-2:6:2-6.7to2-6.12] of real numbers.
- ( is called exclusion bracket
[ is called inclusive bracket
- _keys for comments, however it might be tempting to use this as a parser instruction

- Which informative Regular expression reference to include? This document SHOULD not describe any special type of regular expression.
- How to reference ISO 80000-2 ? in informational?
- Should references include ABNF
- Ask for feedback on a grammatical English level as English is not my mother tongue.

# Implementation:

- How would I implement a validator of JSON Grammar? This is probably an exercise to do while waiting for a response from Tim (if any). How about creating (Python) types for a regex, set, interval, string, boolean, optional, number, integer, etc.. and check the json for those. Use REGEX to validate? Do I need this?
	- I do need this to make validation easy in python, if each type is a class I can just pass in the json value and report anything that doesn’t match as an error.
	- Print warning if string beings with [ but wasn’t 

```python

''' 
switch doesn't work
how about different methods for each or at least the difficult types?
string should probably be the last type

name is null for the root value
use the fact that python probably knows how to map an integer or number from json to python internal. e.g. unwrap from within string and use json.loads to create a python representation to do that. That way there is no need for a regex for a number.
Write the library that does parse sets and integers separately?

'''
def recursive(name,value):
	type = type(value)
	if type == object:
		for each name,value in object:
			recursive(name,value)
	elif type == array:
		for each value in array:
			recursive(name, value)
	elif type == boolean:
			
	elif type == string:

	elif type == number:

	elif type == integer:
		

throw if not valid JSON
jsond = json.loads(text)
recursive(null, jsond)



```