
### Get introduced

JSOND (_may also be written as_ **jsond**) is a way of defining a named json object using _json_. The object being defined is created using json where values are either a nested object, an array with a single value, or a string representing the data type.

Let's recap [JSON](http://json.org) basics. JSON is built on two structures:

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

Based on the valid values JSOND simply defines the following types:

- "string"
- "number"
- "boolean"

It is also possible to use a named json object as a data type, as illustrated in the example below.

Before we get in to the example there are two more points worth mentioning:

1. Arrays must define a single value with the data type they will contain. In JSOND it is not allowed to define different types of data within the same array.

2. JSOND does not allow defining something as null.

### An illustrative example

The following JSOND describes a person with a name, an age, if the person is student or not, a list of hobbies, and who the persons parents are.

**person.json**
```
{
    "name": "string",
    "age": "number",
    "student": "boolean",
    "hobbies": [
        "string"
    ],
    "parents": {
        "father": "person",
        "mother": "person"
    }
}
```

The types "string", "number", and "boolean" is used to describe the first three properties _name_, _age_ and _student_. The property _hobbies_ is defined as a list of strings. The nested parents object is defined as having two properties _father_ and _mother_, both of the type "person". The type "person" is allowed because it is a named json object defined using JSOND.

### A few words on completeness

At this stage, and possibly never, JSOND is not meant to be complete. The purpose for its creation on a rainy thursday was to describe json output from rest endpoints, so that corresponding models in the client could be generated rather than manually created.

