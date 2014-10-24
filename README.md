# Introduction

JSOND is a simple and natural way to define JSON.

The purpose of JSOND is to facilitate documentation of JSON text.

## Conventions Used in This Document

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC2119].

The grammatical rules in this document are to be interpreted as described in [RFC5234].

# JSOND Grammar

JSOND text is JSON text that includes JSOND grammar. JSOND grammar is a superset of JSON grammar [RFC7159]. The rest of this document describes the JSOND grammar.

## Values

A JSOND value MUST be an object, array or a string literal.

	value = object / array / string-literal

§The rest of this section describes string literals, optionals, references and constants.

## Objects

A JSOND object MUST define all members in the corresponding JSON object. A JSON object MUST NOT contain a member that has not been defined in the JSOND object.

## Arrays

A JSOND array MUST define all values in the corresponding JSON array. A JSON array element MUST be defined by at least one of the elements in the JSOND array.

## Booleans

A JSOND “boolean” defines that the JSON value MUST be either true or false.

	string-literal = %x62.6f.6f.6c.65.61.6e	 ; boolean

## Strings

A JSOND “string” defines that the JSON value MUST be a string.

	string-literal = %x73.74.72.69.6e.67	 ; string

Regular expressions [REGEXP] MAY be used to define a set of strings.

	string-literal = regular-expression

## Numbers and Integers

A JSOND “number” defines that the JSON value MUST be a number.

A JSOND “integer” defines that the JSON value MUST be a integer.

	string-literal = %x6e.75.6d.62.65.72 / %x69.6e.74.65.67.65.72 ; number / integer

Mathematical sets and/or intervals MAY be used to define a set of numbers [ISO_80000-2].

	string-literal = *( set / interval )

	set = begin-object [ number *( value-separator number ) ] end-object
	interval = begin-array / begin-parenthesis ( ( number value-separator ) / ( number value-separator number ) / ( value-separator number ) ) end-array / end-parenthesis

	begin-parenthesis = %x28 ; (
	end-parenthesis = %x29	   ; )

	number = integer [ frac ] [ exp ]
	integer = [minus] zero / ( digit1-9 *DIGIT )

An interval that is declared using integers represent the corresponding set of integers. An interval MUST be declared using a number with an explicit decimal component to represent all numbers.

In an interval the left or right element is OPTIONAL. An undefined left element represents negative infinity. An undefined right element represents positive infinity.

The right element SHOULD be greater than the left element. Insignificant whitespace is allowed.

## Optionals

A member can be defined as optional by appending the optional-character to the end of the JSOND name.

	name = string [ optional-character ]
	optional-character = %x3f ; ?

An optional member MAY have the value null. An optional member MAY be undefined in JSON text.

### References

A JSOND value MAY be stored in a file. A file can be referenced using the file or http(s) protocol [RFC3986].

	string-literal = [ protocol ] file

The protocol is OPTIONAL if the value can be evaluated as a relative path to a JSOND file.

### Constants

A JSOND string literal value that is not valid JSOND grammar SHOULD be interpreted as a value constant and thus REQUIRED in JSON text.

Valid JSON text is also valid JSOND text.

## References

### Normative References

- [RFC7159]  Bray, T., "The JavaScript Object Notation (JSON) Data Interchange Format", RFC 7159, March 2014.
- [RFC5234]  Crocker, D. and P. Overell, "Augmented BNF for Syntax Specifications: ABNF", STD 68, RFC 5234, January 2008.
- [RFC3986]  Berners-Lee, T., Fielding R., and Masinter, L., "Uniform Resource Identifier (URI): Generic Syntax", RFC 3986, January 2005.
- [RFC2119]  Bradner, S., "Key words for use in RFCs to Indicate Requirement Levels", BCP 14, RFC 2119, March 1997.

### Informative References

- [ECMA-404]  Ecma International, "The JSON Data Interchange Format", Standard ECMA-404, October 2013, <http://www.ecma-international.org/publications/standards/Ecma-404.htm>.
- [RFC4627]  Crockford, D., "The application/json Media Type for JavaScript Object Notation (JSON)", RFC 4627, July 2006.
- [ISO_80000-2] http://www.iso.org/iso/home/store/catalogue_tc/catalogue_tc_browse.htm?commid=46202
- [REGEX] http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap09.html

## Appendix: Examples

// Honoring a person with the example gives it an extra touch
// make it timeless

{
	“name”: “Stephen Kleene”,
	“lifetime”: “[1909,1994]”,
	“pi”: “[3.14,3.15)”

}

  {
    "Precision":"string:^zip$",
    "Latitude":"number:[-90,90]",
    "Longitude":"number:[0,180]",
    "Address":"string",
    "City":"string",
    "State":"string:\w{2}",
    "Zip":"string:\d{5}",
    "Country":"string:\w{2}"
  }

```jsond
[
	“boolean”,
	“string”,
	[
		“number”
	],
]
```

```
[
	“it’s”,
	true,
	“this array MAY contain”,
	[
		1, 2, 2.5, 3
	]
	1,
	2,
	3,
	4.5,
	“any value that is not a
]
```

{
	“name”:”string”,
	“

}

This is JSOND text:

```
string
boolean
number
integer
set x 2
interval x 2
array x 2

```jsond
{
	name: string
}
```


```jsond
{
	name: null
}
```

```jsond
{
}
```


```
{
  "image": {
    "title":    "^View from \d+th Floor$",
    "width":    "[200.0,)",
    "height":   "[100.0,)",
    "thumbnail": {
      "url":    "http://json.org/url.jsond",
      "height": 125,
      "width":  100
    }
  },
  "addresses": [
    {
      "precision": "zip",
      "latitude":  "number",
      "longitude": "number",
      "address":   "string",
      "city":      "[A-Z]+",
      "state":     "\w\w",
      "zip":       "\d{5}",
      "country":   "US"
    }
  ]
}
```

```json
{
  "Image":{
      "Width":"number:{0,}",
      "Height":"number:{0,}",
      "Title":"string:\w{2,}",
      "Thumbnail":{
        "Url":"string:http://www.TODO:regex.com/image/481989943",
        "Height":"string:\d{3}",
        "Width":"string:\d{3}"
      },
      "IDs":[
        "number:{0,}"
      ]
  }
}
```

```json
[
  {
    "Precision":"string:^zip$",
    "Latitude":"number:[-90,90]",
    "Longitude":"number:[0,180]",
    "Address":"string",
    "City":"string",
    "State":"string:\w{2}",
    "Zip":"string:\d{5}",
    "Country":"string:\w{2}"
  }
]
```


# Notes:

- Plural vs singular (try to use singular)
- Which terms should be used and avoided? declared, represents, element, member, key/value, name/value, structure, corresponding, value types, value definitions … (go through these)
- Types are inferred

- Which informative Regular expression reference to include? This document SHOULD not describe any special type of regular expression.
- How to reference ISO 80000-2 ? in informational?
- Should references include ABNF
