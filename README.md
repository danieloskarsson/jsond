## Introduction

JSOND is a simple and natural way to define JSON.

The purpose of JSOND is to facilitate documentation of JSON text.

JSOND text is JSON text that uses JSOND grammar. JSOND grammar is a superset of JSON grammar. The rest of this document describes the JSOND grammar.

### Conventions Used in This Document

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC2119].

The grammatical rules in this document are to be interpreted as described in [RFC4234].

## Grammar

### Value Types

To define a set of JSON values, a JSOND value is a string literals that denotes the primitive type.

`value = string / boolean / number / integer`

Integer is not a primitive type in JSON. It is included in JSOND for pragmatic reasons.

### Value Definitions

JSOND supports defining the value of strings using regular expressions [REF] and numbers using the set or interval notation [REF] as string literals.

`value = regular-expression / set-notation / interval-notation`

`set-notation = begin-object [ number *( value-separator number ) ] end-object`

`interval-notation = begin-array / begin-parenthesis ( ( number value-separator ) / ( number value-separator number ) / ( value-`separator number ) ) end-array / end-parenthesis`

`number = integer [ frac ] [ exp ]`
`integer = [minus] zero / ( digit1-9 *DIGIT )`

`begin-parenthesis = %x28 ; (`
`end-parenthesis = %x29	 ; )`

Insignificant whitespace is allowed in both the set and interval notation.

In intervals, the left or right number MAY be omitted. An omitted number represents infinity. Unless the left and/or the right number is explicitly specified using a fraction part and/or an exponent part only integers are considered in that interval. If both the left and right number is specified the right number must be greater than the right number.

Value types are inferred from value definitions.

### Arrays

JSON arrays MUST consist of zero or more of the structures defined in the JSOND array.

### Value References

JSOND values MAY be stored in a file. Files can be referenced using the file or http protocol [REF]. The protocol is optional if the value can be evaluated as a relative path to a JSOND file.

### Value Constants

A value that not valid JSOND grammar is REQUIRED in the JSON text.

### Optional Pair

A name/value pair can be defined as optional in the JSON text by appending the optional character to the end of the name.

`name = string [ optional-character ]`
`optional-character = %x3f ; ?`

An optional pair is either not part of the JSON text or has a value of null.

## Examples

This is JSOND text:

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


## References

### Normative References

- [RFC7159]  Bray, T., "The JavaScript Object Notation (JSON) Data Interchange Format", RFC 7159, March 2014.
- [RFC5234]  Crocker, D. and P. Overell, "Augmented BNF for Syntax Specifications: ABNF", STD 68, RFC 5234, January 2008.
- [RFC3986]  Berners-Lee, T., Fielding R., and Masinter, L., "Uniform Resource Identifier (URI): Generic Syntax", RFC3986, January 2005.
- [RFC2119]  Bradner, S., "Key words for use in RFCs to Indicate Requirement Levels", BCP 14, RFC 2119, March 1997.

### Informative References

- [ECMA-404]  Ecma International, "The JSON Data Interchange Format", Standard ECMA-404, October 2013, <http://www.ecma-international.org/publications/standards/Ecma-404.htm>.
- [RFC4627]  Crockford, D., "The application/json Media Type for JavaScript Object Notation (JSON)", RFC 4627, July 2006.
