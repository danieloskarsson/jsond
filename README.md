# §Introduction

JSOND is a natural way to define JSON.

The purpose of JSOND is to facilitate documentation.

JSOND is designed to be a minimal superset of JSON.

XXX:

JSOND (JSON Definition) is a simple and elegant/natural way to define JSON text. The purpose is to aim the process of defining JSON e.g. by writing it on a whiteboard or using a text editor.

JSOND, is a way of defining JSON using JSON. The structure being defined is created using JSON where values are either a nested object, an array, or a string defining the type of data.

JSON Definition (JSOND) is a language for defining a JSON structure using a JSON structure formatted according to a few simple rules described in this document.

JSON Definition (JSOND) is a natural way of defining JSON using JSON.

It is a simple, yet powerful, definition language conceived to facilitate discussions, replace the use of examples, and enable model generation and validation of JSON structures.

It has been designed to be a simple and practical way to define JSON on a whiteboard or in documentation.


JSOND was created to support the process of defining JSON structures and replace the use of JSON examples in API documentation, but can also be used to validate existing JSON and generate models in any programming language that support the corresponding JSON data types.

JSOND's design goals were for the definitions not only to be valid JSON, but also have the same structure as the JSON structure being defined.

JSON's design goals were for it to be minimal, portable, textual, and a subset of JavaScript.


:XXX

## Conventions Used in This Document

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC2119].

The grammatical rules in this document are to be interpreted as described in [RFC5234].

# §JSOND Grammar

JSOND text is JSON text that MAY include JSOND grammar. JSOND grammar is a superset of JSON grammar [RFC7159]. The rest of this document describes the JSOND grammar.

§The JSON structure being defined is itself a JSON structure.

§JSOND is a superset of JSON. This means that valid JSON text is also valid JSOND text. This also means that valid JSOND text is valid JSON text and can thus be parsed using any JSON parser. Context decides if the text is JSON or JSOND.

## Values

A JSOND value MUST be an object, array or a string literal.

	value = object / array / string-literal

## Objects

A JSOND object MUST define all members in the corresponding JSON object. A JSON object MUST NOT contain a member that has not been defined in the JSOND object.

## Arrays

A JSOND array MUST define a list/sequence of values in such way that all corresponding JSON array values are defined by at least one of the values in the JSOND array.

## Booleans

A JSOND "boolean" defines that the JSON value MUST be either true or false.

	string-literal = boolean
	boolean = %x62.6f.6f.6c.65.61.6e	 ; boolean

## Strings

A JSOND "string" defines that the JSON value MUST be any string.

	string-literal = string
	string = %x73.74.72.69.6e.67	     ; string

Regular expressions [REGEX] MAY be used to define a subset of strings.

	string-literal = regular-expression

## §Numbers and Integers

A JSOND "number" defines that the JSON value MUST be any number.

A JSOND "integer" defines that the JSON value MUST be any integer.

	string-literal = number / integer
	number = %x6e.75.6d.62.65.72     ; number
	integer = %x69.6e.74.65.67.65.72	 ; integer

Mathematical sets and/or intervals [ISO-80000-2] MAY be used to define a subset of numbers.

	string-literal = *( set / interval )

	set = begin-object [ number-literal *( value-separator number-literal ) ] end-object

	interval = begin-array / begin-parenthesis ( ( number-literal value-separator ) / ( number-literal value-separator number-literal ) / ( value-separator number-literal ) ) end-array / end-parenthesis

	begin-parenthesis = %x28	 ; (
	end-parenthesis = %x29   ; )

	number-literal = integer-literal [ frac ] [ exp ]
	integer-literal = [minus] zero / ( digit1-9 *DIGIT )

An interval that is declared using only integers defines a subset of integers. An interval that is declared with at least one number with an explicit decimal component defines a subset of numbers.

§An interval that is declared using integers defines the corresponding subset of integers. An interval MUST be declared using at least one number with an explicit decimal component to define a subset of real numbers.

In an interval the left or right element is OPTIONAL. An undefined left element defines negative infinity. An undefined right element defines positive infinity.

§there might but shouldn't be a problem in sending numbers with .0 (libraries might remove them!)

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

### §References

A JSOND value MAY be stored in a file. A file MUST be referenced using the file or http(s) protocol [RFC3986].

	string-literal = [ protocol ] file

The protocol is OPTIONAL if the value can be evaluated as a relative path to a JSOND file.


§ and referenced from other JSOND files as if it's content was included in the referencing document.

§Definitions can be reused by referencing a JSOND file using the http or file protocol. JSOND files should have the file suffix .jsond and the mime-type application/jsond.

### §Constants

A value that is not valid JSOND grammar SHOULD be interpreted as a value constant and is thus REQUIRED in JSON text.

XXX:
It should also be noticed that while not being a JSOND value null, true, false and numbers are interpreted by JSOND validators.

Most (§all?) string literals are valid regular expressions.

This might be hard to actually have working since almost every expression is a regular expression. For normal things that is the same thing. For other strings that contain regular expression special things, this will be the most common error.

It actually works for null, true, false and such things

A JSOND document that is valid JSON is never invalid. Each member is evaluated separately and if a value does not make sense to JSOND, e.g. a invalid regular expression, the value for the name is considered to be a string that consists of exactly that invalid regular expression.

A JSOND parser SHOULD warn for this.
:XXX

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

## §Appendix: Examples

Basic use of JSOND is as simple as replacing literal example values with string literals that define the value type.

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

§The example below defines a list of products.

Values can be constrained …

More advanced use of JSOND …

Types are inferred.

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

In the example above id MUST be any integer greater than or equal to zero, name MUST only consist of letters a-z and digits 0-9, category MUST be category1 or category2, price MUST be any positive number greater than zero, reduced member MUST be true, false, null or undefined, url is defined by the referenced url.jsond.

```url.jsond
"^https?:// \.[a-z]{2,}"
```


url -> date
name -> slug
category -> set
constant

[1,5]{7,9}

