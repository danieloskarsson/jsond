# Introduction

JSOND is the natural way to define JSON.

The purpose of JSOND is to facilitate documentation.

JSOND is designed to be a minimal superset of JSON.

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

An interval that is declared using integers defines the corresponding set of integers. An interval MUST be declared using a number with an explicit decimal component to define all numbers.

In an interval the left or right element is OPTIONAL. An undefined left element defines negative infinity. An undefined right element defines positive infinity.

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

## Appendix: Examples

When defining JSON focus SHOULD be on defining the objects and members and it is RECOMMENDED to omit the details. The example below defines a list of products.

```
[
	{
		"id": "integer",
		"name": "string",
		"category": "string",
		"price": "number",
		"onsale": "boolean",
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
		"category": "(category1|category2)",
		"price": "(0.0,)",
		"onsale?": "boolean",
		"url": "http://jsond.org/url.jsond"
	}
]
```

In the example above the id MUST NOT be a negative integer, the name MUST only consist of letters a-z and digits 0-9, the category MUST be category1 or category2, the price MUST be any positive number greater than zero, the on sale member may be true, false, null or undefined in json text, the url is defined by the JSOND value in url.jsond, which requires that the url starts with http.

