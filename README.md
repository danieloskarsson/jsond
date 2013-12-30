
## Introduction

JSOND, _may also be written as_ jsond, is a way of defining JSON using JSON. The structure being defined is created using JSON where values are either a nested object, an array, or a string defining the type of data.

### JSON

Let's recap JSON basics. JSON is built on two structures:

- A unordered set of name:value pairs (object)
- An ordered list of values (array)

In JSON, a value must be one of the following:

- object
- array
- string
- number
- true
- false
- null

### Types

Based on the valid values JSOND simply defines the following types:

- "string"
- "number"
- "boolean"

JSOND is simply normal JSON where the values are either




 taking JSON and replacing the values with one of the types listed above.

### An illustrative example

The following JSOND defines a person with a name, an age, whether the person is student or not, the persons favorite artist and team, and a list of hobbies.

**person.jsond**
```
{
    "name": "string",
    "age": "number",
    "student": "boolean",
    "favorites": {
		"artist": "string",
		"team": "string"
    },
    "hobbies": []
}
```

The types **string**, **number**, and **boolean** is used to define the first three properties _name_, _age_ and _student_. The nested _favorites_ object is defined as having two properties _artist_ and _team_, both of the type **string**. The property _hobbies_ is defined as an array.

### Optional properties

Properties can be defined as optional. Below the previous example has been enhanced so that the age number, favorites object and hobbies array are optional.

**person.jsond**
```
{
    "name": "string",
    "age:optional": "number",
    "student": "boolean",
    "favorites:optional": {
	"artist": "string",
	"team": "string"
    },
    "hobbies:optional": []
}
```

Properties that are defined as optional may either be missing, defined as null, or contain a value of the defined type.

### Custom types

As long as JSOND can parse the type, any user-defined JSOND is valid as type. The name of the JSOND structure is used as the type name. In the example below favorites has been defined in a structure named favorites.

**favorites.jsond**
```
{
    "artist": "string",
    "team": "string"
}
```

**person.jsond**
```
{
    "name": "string",
    "age": "number",
    "student": "boolean",
    "favorites": "favorites",
    "hobbies": []
}
```

Person has been changed so that the defined type of "favorites" are the same as the name of the structure, which is also "favorites".

Recursive type definitions are allowed.

**person.jsond**
```
{
    "name": "string",
    "age": "number",
    "student": "boolean",
    "favorites": "favorites",
    "hobbies": [],
    "parents": {
        "father": "person",
        "mother": "person"
    }
}
```

In the example above there is an additional object defined with the name "parents". The properties of the object is father and mother which is defined as having the type "person", which is a reference to the name of the overall structure.

## Value definitions

In addition to define types, JSOND can also be used to define valid strings, numbers, and content of arrays. Value definitions appear after the type definition separated with a colon. When no value definition is supplied any value of the correct type is considered as valid value.

### Strings

Strings are value defined using regular expressions.

**status.jsond**
```
{
	"status":"string:^(OK|FAILURE)$",
	"utc":"string:\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}(\.\d{1,6})?Z"
}
```

In the example above the value of status may be either "OK", or "FAILURE" and the value of utc is a valid UTC timestamp with up to six fractions.

### Numbers

Numbers are defined using one or more ranges. A range consists of two numbers separated with a comma within brackets. E.g. {18,99} defines any integer that is equal to or greater than 18 _and_ less than or equal to 99. There are three types of brackets:

- Definite brackets {} are inclusive and defines any _integer_ within the range.
- Closed brackets [] are inclusive and defines any number within the range.
- Circle brackets () are _exclusive_ and defines any number within the range.

Closed and circle brackets can be combined. E.g. [18,19) defines any number that is equal to or greater than 18 _and_ less than 19. This is typically used to describe that any number of decimal fractions is allowed.

A range can be left open by omitting one of the number. E.g. {18,} defines any integer that is equal to or greater than 18.

Multiple ranges can be combined. E.g. (,0),(0,) defines any positive or negative number except zero.

### Arrays

The type of values inside an array can be defined. E.g. ["string"] defines that the array should only consist of strings.

### An enhanced example

The following example is an enhanced version of the previous example, enhanced with value definitions.

**person.jsond**
```
{
    "name": "string:\w{2,}",
    "age": "number:{18,}",
    "student": "boolean",
    "favorites": {
	"artist": "string",
	"team": "string"
    },
    "hobbies": [
        "string"
    ]
}
```

The value of name is defined to contain a string with at least two characters. The age of a person is defined to be at least 18 years old, and cannot be 18.5. The property hobbies is defined as an array of strings.

## Comments

JSOND support comments inside the JSON structure. Comments can be made in relation to both the name and value.

**person.jsond**
```
{
    "name": "string:\w{2,}",
    "age": "number:{18,}:We are only allowed to process users that are at least 18 years old.",
    "student": "boolean",
    "favorites::To be defined in it's own structure.": {
	"artist": "string",
	"team": "string"
    },
    "hobbies": [
        "string::Should we define a list of known hobbies?"
    ]
}
```

In the example above there are three comments:

- _We are only allowed to process users that are at least 18 years old._ on the value of age.
- _To be defined in it's own structure._ on the property name favorites.
- _Should we define a list of known hobbies?_ on the defined value type in hobbies.

As can be seen in the example above the use of comments does not require the use of value definitions.