# Introduction

JSOND (JSON Definition) is a simple, yet powerful, definition language for JSON text.

The purpose of JSOND is to facilitate development and documentation of JSON text.

JSOND is designed to be a minimal superset of JSON.

## Conventions Used in This Document

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC2119].

The grammatical rules in this document are to be interpreted as described in [RFC5234].

# JSOND Grammar

JSOND text is JSON text that MAY include JSOND grammar. JSOND grammar is a superset of JSON grammar [RFC7159] [RFC4627] [ECMA-404]. The rest of this document describes the JSOND grammar.

## Values

A JSOND value MUST be any JSON object, array or string.

	value = object / array / string

## Objects

A JSOND object MUST define all members in the corresponding JSON object. A JSON object MUST NOT contain a member that has not been defined in the corresponding JSOND object.

	value = object                           ; e.g. { "name": "string" } 

## Arrays

A JSOND array MUST define a sequence of values in such way that all corresponding JSON array values are defined by at least one of the values in the JSOND array.

	value = array                            ; e.g. [ "boolean", "string" ]

## Booleans

A JSOND boolean defines that the corresponding JSON value MUST be true or false.

	value = %x22.62.6f.6f.6c.65.61.6e.22     ; "boolean"

## Strings

A JSOND string defines that the corresponding JSON value MUST be any string.

	value = %x22.73.74.72.69.6e.67.22        ; "string"

Regular expressions [ECMA-262] MAY be used to define a subset of strings.

	value = %x22 regular-expression %x22     ; e.g. "[a-z]"

## Numbers and Integers

A JSOND number defines that the corresponding JSON value MUST be any number.

	value = %x22.6e.75.6d.62.65.72.22        ; "number"

A JSOND integer defines that the corresponding JSON value MUST be any integer.

	value = %x22.69.6e.74.65.67.65.72.22     ; "integer"

### Sets and Intervals

An arbitrary number of mathematical sets and intervals [ISO-80000-2] MAY be used to define a subset of numbers.

A corresponding JSON number MUST match a number in the defined subset.

	begin-exclusive = %x28                   ; (

	end-exclusive = %x29                     ; )

	integer = [ minus ] zero / ( digit1-9 *DIGIT )

	number = integer [ frac ] [ exp ]

	set = begin-object number *( value-separator number ) end-object

	interval = begin-array / begin-exclusive ( ( number value-separator ) / ( number value-separator number ) / ( value-separator number ) ) end-array / end-exclusive

	value = %x22 ( set / interval ) *( set / interval ) %x22

Set elements SHOULD be ordered in increasing order from the least to the greatest element. There must be at least one element.

An interval that is declared using integers defines the corresponding subset of integers. An interval MUST be declared using at least one number with an explicit decimal component to define a subset of real numbers. The decimal component MAY be .0.

In an interval the left or right endpoint is OPTIONAL. An undefined left endpoint defines negative infinity. An undefined right endpoint defines positive infinity. If both endpoints are provided the left  endpoint MUST be less than the right endpoint.

Insignificant whitespace is OPTIONAL in sets and intervals.

## References

Any JSOND value MAY be persisted as a file. A file SHOULD be referenced using a relative path, an absolute path, or using the http or https scheme [RFC3986].

	value = %x22 [ scheme ] path %x22        ; e.g. "file.jsond"

JSOND files SHOULD have the filename extension .jsond.

Circular references SHOULD be avoided.

## Constants

A value that is not valid JSOND grammar SHOULD be interpreted as a constant and thus REQUIRED in the corresponding JSON text.

The literals, true, false, and null are not valid JSOND grammar. Numbers are not valid JSOND grammar.

Most strings are valid regular expressions and thus valid JSOND grammar. String constants SHOULD include boundary matchers.

## Optionals

A member can be defined as optional by appending the optional-character to the end of the JSOND name.

	optional-character = %x3f                ; ?

	name = name [ optional-character ]       ; e.g. name?

An optional member MAY have the value null. An optional member MAY be undefined in JSON text.

# References

## Normative References

- [RFC7159]  Bray, T., "The JavaScript Object Notation (JSON) Data Interchange Format", RFC 7159, March 2014.
- [RFC5234]  Crocker, D. and P. Overell, "Augmented BNF for Syntax Specifications: ABNF", STD 68, RFC 5234, January 2008.
- [RFC3986]  Berners-Lee, T., Fielding R., and Masinter, L., "Uniform Resource Identifier (URI): Generic Syntax", RFC 3986, January 2005.
- [RFC2119]  Bradner, S., "Key words for use in RFCs to Indicate Requirement Levels", BCP 14, RFC 2119, March 1997.

## Informative References

- [ECMA-404]  Ecma International, "The JSON Data Interchange Format", Standard ECMA-404, October 2013, <http://www.ecma-international.org/publications/standards/Ecma-404.htm>.
- [ECMA-262]  Ecma International, "ECMAScript® Language Specification", Standard ECMA-262, June 2011, <http://www.ecma-international.org/publications/standards/Ecma-262.htm>.
- [ISO-80000-2]  International Organization for Standardization, "Quantities and units — Part 2: Mathematical signs and symbols to be used in the natural sciences and technology", Standard ISO 80000-2:2009, December 2009, <http://www.iso.org/iso/home/store/catalogue_tc/catalogue_tc_browse.htm?commid=46202>.
- [RFC4627]  Crockford, D., "The application/json Media Type for JavaScript Object Notation (JSON)", RFC 4627, July 2006.

# Appendix: Examples

Basic use of JSOND use JSON strings "boolean", "string", "number", and "integer" as values as in Example 1.

```
[
	{
		"id": "integer",
		"slug": "string",
		"url": "string"
		"category": "integer",
		"price": "number",
		"reduced": "boolean",
	}
]
```

_Example 1: Basic JSOND that defines a sequence of zero or more products._

Example 1 defines that for each product in the sequence:

- id MUST be any integer
- slug MUST be any string
- url MUST be any string
- category MUST be any integer
- price MUST be any number
- reduced MUST be true or false

Advanced use of JSOND MAY include use of regular expressions, mathematical sets and intervals, optionals, references, and constants as values as in Example 2.

```
[
	{
		"id": "[0,)",
		"slug": "[a-z0-9]",
		"url": "http://jsond.org/url.jsond"
		"category": "{10,25,50}",
		"price": "(0.0,)",
		"reduced?": "boolean",
		"margin": "(high|medium|low)",
		"available": true,
	}
]
```
_Example 2a: Advanced JSOND that defines a sequence of zero or more products._

Example 2a defines that for each product in the sequence:

- id MUST be any integer greater than or equal to zero
- slug MUST only consist of letters a-z and digits 0-9
- url is defined by the referenced file url.jsond
- category MUST be 10, 25, or 50
- price MUST be any positive number greater than zero
- reduced MUST be true, false, null or undefined
- margin MUST be high, medium, or low
- available MUST be true

The value for url is defined in url.jsond in Example 2b.

```
"^https?://[^\.]+\.[a-z]{2,}"
```
_Example 2b: url.jsond_
