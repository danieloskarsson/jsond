# §Introduction
## Conventions Used in This Document
# §JSOND Grammar
## §Values
## §Objects
## §Arrays
## §Booleans
## §Strings
## §Numbers and Integers
## Nulls
## Optional Members
## §Value References
## §Constants
# References
## Normative References
## Informative References
# §Appendix A: Examples


# Introduction

JSOND is a simple and natural way to define JSON.

§The purpose of JSOND is to facilitate documentation of JSON text.

§The design goal of JSOND is to be a lightweight easy JSON-like way to define … merge with above

## Conventions Used in This Document

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC2119].

The grammatical rules in this document are to be interpreted as described in [RFC4234].

# JSOND Grammar

JSOND text is JSON text that includes JSOND grammar. JSOND grammar is a superset of JSON grammar. The rest of this document describes the JSOND grammar.

## Values

A JSOND value MUST be an object, array or a string literal.

	value = object / array / string-literal

No other values are allowed. / Other values are considered constants.



The string literal MAY be used to define the value type: boolean, string, number, or integer.

	boolean = %x62.6f.6f.6c.65.61.6e		; boolean
	string = %x73.74.72.69.6e.67				; string
	number = %x6e.75.6d.62.65.72				; number
	integer = %x69.6e.74.65.67.65.72		; integer




If MAY OPTIONALLY be used to 

The string literal is either used to define the value type, or to 

A JSOND value MUST be an object, array, one of the string literals “boolean”, “string”, “number”, or “integer”, a regular expression, or an arbitrary number of set and/or intervals.

	value = object / array / boolean / string / number / integer / regular-expression, set, interval

	boolean = %x62.6f.6f.6c.65.61.6e		; boolean
	string = %x73.74.72.69.6e.67				; string
	number = %x6e.75.6d.62.65.72				; number
	integer = %x69.6e.74.65.67.65.72		; integer

Regular expressions, sets and intervals are expressed as string literals.

JSOND supports defining the value of strings using regular expressions [REF] and numbers using the set or interval notation [REF] as string literals.


To define a set of JSON values, a JSOND value is a string literals that denotes the primitive type.

`value = string / boolean / number / integer`


## Objects

A JSOND object MUST define all members in the corresponding JSON object.

_describe the structure similaries between JSON text and JSOND text_

## Arrays

JSOND array elements define values in the corresponding JSON array. A JSON array element MUST be defined by at least one of the elements in the corresponding JSOND array.

There are no bounds on the number of JSON array element occurrences.

## Booleans

A JSOND boolean defines that the JSON value MUST be either true or false.

A JSOND boolean is defined using the string literal “boolean”.

A JSOND boolean defines that the corresponding JSON value MUST be either true or false.

	value = boolean
	boolean = %x62.6f.6f.6c.65.61.6e		; boolean

## Strings

A JSOND string defines that the JSON value MUST be a string literal.

value = regular-expression


## Numbers and Integers

Numbers and Integers can also be defined using an arbitrary union of set and intervals.

	value = *( set / interval )

	set = begin-object [ number *( value-separator number ) ] end-object
	interval = begin-array / begin-parenthesis ( ( number value-separator ) / ( number value-separator number ) / ( value-separator number ) ) end-array / end-parenthesis

Intervals MUST only consist of integers unless 


XXX: Require zero, one, two elements in a set? <- why?

value = [


JSON treat all 

Numbers are defined using the string literal “number”. 

support multiple integer notations

UNION/

an arbitrary number of interval notation and/or a set. ¨
(as opposed to set or interval notation)


value = regular-expression / set / interval 

set = begin-object [ number *( value-separator number ) ] end-object

interval = begin-array / begin-parenthesis ( ( number value-separator ) / ( number value-separator number ) / ( value-separator number ) ) end-array / end-parenthesis

number = integer [ frac ] [ exp ]
integer = [minus] zero / ( digit1-9 *DIGIT )

begin-parenthesis = %x28 ; (
end-parenthesis = %x29	 ; )

**Insignificant whitespace is allowed in both the set and interval notation.**

Multiple notations are allowed.

In intervals, the left or right number MAY be omitted. An omitted number represents infinity. Unless the left and/or the right number is explicitly specified using a fraction part and/or an exponent part only integers are considered in that interval. If both the left and right number is specified the right number must be greater than the right number.

Value types are inferred from value definitions.

Integer is not a primitive type in JSON. It is included in JSOND for pragmatic reasons.

## Nulls

A value MUST NOT be null unless the member has been defined as optional.

## Optional Members

A member can be defined as optional by appending the optional-character to the end of the JSOND name.

	name = string [ optional-character ]

	optional-character = %x3f ; ?

An optional member MAY have the value null. An optional member MAY be undefined in JSON text.

### Value References

A JSOND value MAY be referenced from a file.  …

JSOND values MAY be stored in files. Files can be referenced using the file or http protocol [REF]. The protocol is OPTIONAL if the value can be evaluated as a relative path to a JSOND file.

### Value Constants

A JSOND value that is not valid JSOND grammar is a value constant and thus REQUIRED in JSON text.

_All JSON text is valid JSOND text._
_JSON can thus be used to validate itself :-)_

## References

### Normative References

- [RFC7159]  Bray, T., "The JavaScript Object Notation (JSON) Data Interchange Format", RFC 7159, March 2014.
- [RFC5234]  Crocker, D. and P. Overell, "Augmented BNF for Syntax Specifications: ABNF", STD 68, RFC 5234, January 2008.
- [RFC3986]  Berners-Lee, T., Fielding R., and Masinter, L., "Uniform Resource Identifier (URI): Generic Syntax", RFC3986, January 2005.
- [RFC2119]  Bradner, S., "Key words for use in RFCs to Indicate Requirement Levels", BCP 14, RFC 2119, March 1997.
RFC4234

### Informative References

- [ECMA-404]  Ecma International, "The JSON Data Interchange Format", Standard ECMA-404, October 2013, <http://www.ecma-international.org/publications/standards/Ecma-404.htm>.
- [RFC4627]  Crockford, D., "The application/json Media Type for JavaScript Object Notation (JSON)", RFC 4627, July 2006.

## Appendix: Examples

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

- Plural vs singular
- Which terms should be used and avoided? element, member, key/value, name/value, structure, …
