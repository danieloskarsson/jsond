
# Notes:

- OVERRIDE build in types by creating a file named "string", "boolean", etc…? SHOULD probably not be allowed. I.e. use .jsond to make it explicit that an explicit type is used. This is a reason for WHY you should use .jsond for custom types/references.
	- "There are four build in types "string", "integer", "number", "boolean", it is possible to define your own types. These should be stored in files with the .jsond suffix."
- Plural vs singular (try to use singular)
- Which terms should be used and avoided? +declared, ~~represents~~, +element, +member, ~~key/value~~, +name/value, ~~structure~~, +corresponding, ~~value types, value definitions~~, allowed(1), +undefined, +null, sequence …
- Types are inferred
- Discuss circular dependencies with references
- Is it possible to write JSOND in JSOND? E.g. to define the type of JSOND that can be sent to do a search.
- In JSOND numbers are considered to be Integers unless a decimal component is explicitly defined.
- It is possible to further define the real number using a set [80000-2:5:2-5.3] of real numbers, or interval [80000-2:6:2-6.7to2-6.12] of real numbers.
- ( is called exclusion bracket
[ is called inclusive bracket
- _keys for comments, however it might be tempting to use this as a parser instruction
"tags?!":["string"]
"created":"http://jsond.org/utc.jsond"
Q: defintion vs description language A: definition
undefined <-javascript
null <-json
optional <-english
skip the .jsond filename? as in "$ref": "http://json-schema.org/geo"
Sending JSOND to a system as a way to search.
JSOND for Configuration for a system of some kind.
- talk about strings instead of string literals?

- Which informative Regular expression reference to include? This document SHOULD not describe any special type of regular expression.
- How to reference ISO 80000-2 ? in informational?
- Should references include ABNF
- Ask for feedback on a grammatical English level as English is not my mother tongue.

# Implementation:

- How would I implement a validator of JSON Grammar? This is probably an exercise to do while waiting for a response (if any). How about creating (Python) types for a regex, set, interval, string, boolean, optional, number, integer, etc.. and check the json for those. Use REGEX to validate? Do I need this?
	- I do need this to make validation easy in python, if each type is a class I can just pass in the json value and report anything that doesn’t match as an error.
	- Print warning if string beings with [ but wasn’t 

```python

''' 
switch doesn't work
how about different methods for each or at least the difficult types?
string should probably be the last type

name is null for the root value
use the fact that python probably knows how to map an integer or number from json to python internal. e.g. unwrap from within string and use json.loads to create a python representation to do that. That way there is no need for a regex for a number.
Write the library that does parse sets and integers separately?

'''
def recursive(name,value):
	type = type(value)
	if type == object:
		for each name,value in object:
			recursive(name,value)
	elif type == array:
		for each value in array:
			recursive(name, value)
	elif type == boolean:
			
	elif type == string:

	elif type == number:

	elif type == integer:
		

throw if not valid JSON
jsond = json.loads(text)
recursive(null, jsond)



```



### Types

Based on the valid values JSOND simply defines the following types:

- `"string"`
- `"number"`
- `"boolean"`

JSOND has the characteristic that it has the exact same structure as the object being defined. The difference between an JSON example and valid JSOND is that example values has been replaced with one of the defined types.

## Value definitions

In addition to define types, JSOND can also be used to define valid strings, numbers, and content of arrays. Value definitions appear after the type definition separated with a colon. When no value definition is supplied any value of the correct type is considered as valid value.



JSOND works so that JSON is written but instead of using examples to illustrate potential values one of the three strings literals: "boolean", "string", "number" is used as the value.

The difference between JSOND and JSON is that JSOND

JSOND values

To define JSON instead of using examples to illustrate potential values JSOND allows you to use any of the following:

- The string literal "boolean"
- The string literal "string"
- The string literal "number"

- A regular expression
- A (mathematical) interval
- A (mathematical) set

- A JSOND reference

Values that does not match any of the JSOND provided values should be seen as the defining the only possible value for that name.


JSOND names

JSOND lists

Tips: When writing JSOND on a whiteboard the double quotes can be left out. This is not valid JSON so it's not valid JSOND either but it's a great way to save time when brainstorming.

JSON values can be mixed in JSOND text, see constants.

JSOND definitions must be syntactical correct to be special to JSOND.

A string literal that is not syntactically JSOND will be treated as a string literal.



redraw the diagrams using only black and white? it looks to much JSON with the current style of the website



When the instance value is a string, this provides a regular
   expression that a string instance MUST match in order to be valid.
   

make sure to read and understand all of json schema
integers was introduced due to much use in practice. it also helps that interval being real numbers or integers is defined by the endpoint declarations
there is no such thing as an integer type in json
there are no comments in json

regextips
For example, the following will match "a" if "a" is not followed by "b".

a(?!b)

:(?!\\)


The backslash \ is an escape character in Java Strings. That means backslash has a predefined meaning in Java. You have to use double backslash \\ to define a single backslash. If you want to define \w, then you must be using \\w in your regex. If you want to use backslash as a literal, you have to type \\\\ as \ is also an escape character in regular expressions.

above section also true in jsond!




Interact with examplessidan för att leka med jsond. Validate json? Generate json/d?








{
  "percent":"[0,100]"
  "percent":"[0.0,1.0]"
  "binary":"[0,1]"
}


An example of corresponding JSON.

```
[
	{
		"id": 1,
		"name": "product1",
		"category": "categoryOne"
		"price": "1.25",
		"onsale": "true",
		"url": "www.example.com/product1"
	},
	{
		"id": 2,
		"name": "product2",
		"category": "category2"
		"price": "5.99",
		"onsale": "false",
		"url": "www.example.com/product2"
	}
]
```
