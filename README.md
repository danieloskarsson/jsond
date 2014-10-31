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

# JSOND Grammar

JSOND text is JSON text that MAY include JSOND grammar. JSOND grammar is a superset of JSON grammar [RFC7159]. The rest of this document describes the JSOND grammar.

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

## Numbers and Integers

A JSOND "number" defines that the JSON value MUST be any number.

A JSOND "integer" defines that the JSON value MUST be any integer.

	string-literal = number / integer

	number = %x6e.75.6d.62.65.72     ; number

	integer = %x69.6e.74.65.67.65.72	 ; integer

An arbitrary number of mathematical sets and intervals [ISO-80000-2] MAY be used to define a subset of numbers.

	string-literal = *( set / interval )

	set = begin-object [ number-literal *( value-separator number-literal ) ] end-object

	interval = begin-array / begin-parenthesis ( ( number-literal value-separator ) / ( number-literal value-separator number-literal ) / ( value-separator number-literal ) ) end-array / end-parenthesis

	number-literal = integer-literal [ frac ] [ exp ]

	integer-literal = [minus] zero / ( digit1-9 *DIGIT )

	begin-parenthesis = %x28	 ; (

	end-parenthesis = %x29   ; )

An interval that is declared using integers defines the corresponding subset of integers. An interval MUST be declared using at least one number with an explicit decimal component to define a subset of real numbers. The decimal component MAY be .0.

In an interval the left or right element is OPTIONAL. An undefined left element defines negative infinity. An undefined right element defines positive infinity.

The right element SHOULD be greater than the left element.

Insignificant whitespace MAY be used before, within, between, and after sets and intervals.

## Optionals

A member can be defined as optional by appending the optional-character to the end of the JSOND name.

	name = name [ optional-character ]
	optional-character = %x3f ; ?

An optional member MAY have the value null. An optional member MAY be undefined in JSON text.

### References

Any JSOND value MAY be persisted as a file. A file MAY be referenced using an relative file path. A file MAY be referenced using an absolute path. A file MAY be referenced using the http or https scheme [RFC3986].

	string-literal = [ scheme ] path

JSOND files SHOULD have the filename extension .jsond.

### §Constants

A value that is not valid JSOND grammar SHOULD be interpreted as a value constant and thus REQUIRED in JSON text.

Most string literals are valid regular expressions.

CONSTANTS :
the only constants are null, true, false

It actually works for null, true, false and such things.

The most common error will be string literals that are interpreted as regular expressions but was misspelled.

A JSOND parser SHOULD warn for this.

§JSOND is a superset of JSON. This means that valid JSON text is also valid JSOND text. This also means that valid JSOND text is valid JSON text and can thus be parsed using any JSON parser. Context decides if the text is JSON or JSOND.

: CONSTANTS

## References

### Normative References

- [RFC7159]  Bray, T., "The JavaScript Object Notation (JSON) Data Interchange Format", RFC 7159, March 2014.
- [RFC5234]  Crocker, D. and P. Overell, "Augmented BNF for Syntax Specifications: ABNF", STD 68, RFC 5234, January 2008.
- [RFC3986]  Berners-Lee, T., Fielding R., and Masinter, L., "Uniform Resource Identifier (URI): Generic Syntax", RFC 3986, January 2005.
- [RFC2119]  Bradner, S., "Key words for use in RFCs to Indicate Requirement Levels", BCP 14, RFC 2119, March 1997.

### §Informative References

- [ECMA-404]  Ecma International, "The JSON Data Interchange Format", Standard ECMA-404, October 2013, <http://www.ecma-international.org/publications/standards/Ecma-404.htm>.
- [RFC4627]  Crockford, D., "The application/json Media Type for JavaScript Object Notation (JSON)", RFC 4627, July 2006.
- [ISO-80000-2] http://www.iso.org/iso/home/store/catalogue_tc/catalogue_tc_browse.htm?commid=46202
- [REGEX] http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap09.html
§IEEE.1003-2.1992 there is no good source to say this is the standard for regex BREX PCEX, ...
§Regular expressions SHOULD follow the regular expression
   specification from ECMA 262/Perl 5

## §Appendix: Examples

Basic use of JSOND is to replace literal example values with "boolean", "string", "number", and "integer" as in Example 1.

```
[
	{
		"id": "integer",
		"slug": "string",
		"category": "string",
		"price": "number",
		"reduced": "boolean",
		"url": "string"
	}
]
```
_Example 1: Basic JSOND that defines a list of products._

Advanced use of JSOND MAY include regular expressions, mathematical sets and intervals, optionals, references, and constants as in Example 2.

```
[
	{
		"id": "[0,)",
		"slug": "[a-z0-9]",
		"category": "{10,25,50}",
		"price": "(0.0,)",
		"reduced?": "boolean",
		"constant": true,
		"url": "http://jsond.org/url.jsond"
	}
]
```
_Example 2: Advanced JSOND that defines a list of products._

Example 2 defines that for each product in the list:

- id MUST be any integer greater than or equal to zero
- slug MUST only consist of letters a-z and digits 0-9
- category MUST be either 10, 25, or 50
- price MUST be any positive number greater than zero
- reduced MUST be true, false, null or undefined
- constant MUST be true
- url is defined by the referenced file url.jsond

The value for url is defined in url.jsond in Example 3.

```
"^https?:// \.[a-z]{2,}"
```
_Example 3: url.jsond_

url -> date
constant
remove reference from example 2? to be used in own example

