# Introduction

JSOND (JSON Definition) is a simple, yet powerful, definition language for JSON text.

The purpose of JSOND is to facilitate development and documentation of JSON text.

JSOND is designed to be a minimal superset of JSON.

## Conventions Used in This Document

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC2119].

The grammatical rules in this document are to be interpreted as described in [RFC5234].

# JSOND Grammar

JSOND text is JSON text that MAY include JSOND grammar. JSOND grammar is a superset of JSON grammar [RFC7159][RFC4627][ECMA-404]. The rest of this document describes the JSOND grammar.

## Values

A JSOND value MUST be an object, array or a string literal.

	value = object / array / string-literal

## Objects

A JSOND object MUST define all members in the corresponding JSON object. A JSON object MUST NOT contain a member that has not been defined in the JSOND object.

## Arrays

A JSOND array MUST define a sequence of values in such way that all corresponding JSON array values are defined by at least one of the values in the JSOND array.

## Booleans

A JSOND "boolean" defines that the JSON value MUST be either true or false.

	string-literal = boolean

	boolean = %x62.6f.6f.6c.65.61.6e	 ; boolean

## Strings

A JSOND "string" defines that the JSON value MUST be any string.

	string-literal = string

	string = %x73.74.72.69.6e.67	     ; string

Regular expressions [ECMA-262] MAY be used to define a subset of strings.

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

Set and interval elements SHOULD be ordered in increasing order from the least to the greatest element.

An interval that is declared using integers defines the corresponding subset of integers. An interval MUST be declared using at least one number with an explicit decimal component to define a subset of real numbers. The decimal component MAY be .0.

In an interval the left or right element is OPTIONAL. An undefined left element defines negative infinity. An undefined right element defines positive infinity.

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

### Constants

A value that is not valid JSOND grammar SHOULD be interpreted as a value constant and thus REQUIRED in JSON text.

Most string literals are valid regular expressions. The values true, false, and null are not valid JSOND grammar.

## References

### Normative References

- [RFC7159]  Bray, T., "The JavaScript Object Notation (JSON) Data Interchange Format", RFC 7159, March 2014.
- [RFC5234]  Crocker, D. and P. Overell, "Augmented BNF for Syntax Specifications: ABNF", STD 68, RFC 5234, January 2008.
- [RFC3986]  Berners-Lee, T., Fielding R., and Masinter, L., "Uniform Resource Identifier (URI): Generic Syntax", RFC 3986, January 2005.
- [RFC2119]  Bradner, S., "Key words for use in RFCs to Indicate Requirement Levels", BCP 14, RFC 2119, March 1997.

### Informative References

- [ECMA-404]  Ecma International, "The JSON Data Interchange Format", Standard ECMA-404, October 2013, <http://www.ecma-international.org/publications/standards/Ecma-404.htm>.
- [ECMA-262]  Ecma International, "ECMAScript® Language Specification", Standard ECMA-262, June 2011, <http://www.ecma-international.org/publications/standards/Ecma-262.htm>.
- [ISO-80000-2]  International Organization for Standardization, "Quantities and units — Part 2: Mathematical signs and symbols to be used in the natural sciences and technology", Standard ISO 80000-2:2009, December 2009, <http://www.iso.org/iso/home/store/catalogue_tc/catalogue_tc_browse.htm?commid=46202>.
- [RFC4627]  Crockford, D., "The application/json Media Type for JavaScript Object Notation (JSON)", RFC 4627, July 2006.

## Appendix: Examples

Basic use of JSOND is to use JSOND string literals "boolean", "string", "number", and "integer" as values as in Example 1.

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
_Example 1: Basic JSOND that defines a sequence of zero or more products._

Advanced use of JSOND MAY include use of regular expressions, mathematical sets and intervals, optionals, references, and constants as values as in Example 2.

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
_Example 2: Advanced JSOND that defines a sequence of zero or more products._

Example 2 defines that for each product in the sequence:

- id MUST be any integer greater than or equal to zero
- slug MUST only consist of letters a-z and digits 0-9
- category MUST be either 10, 25, or 50
- price MUST be any positive number greater than zero
- reduced MUST be true, false, null or undefined
- constant MUST be true
- url is defined by the referenced file url.jsond

The value for url is defined in url.jsond in Example 3.

```
"^https?://[^\.]+\.[a-z]{2,}"
```
_Example 3: url.jsond_
