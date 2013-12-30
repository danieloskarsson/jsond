## enforce (this one is an interesting approach to not repeat myself to much)

Diagram(
Choice(0,Sequence(Sequence('N',','),Choice(0,Skip(),'N')), Sequence(',','N'))
)

-?(?:0|[1-9]\d*)(?:\.\d+)?(?:[eE][+-]?\d+)?

http://stackoverflow.com/questions/13340717/json-numbers-regular-expression
https://www.debuggex.com/
http://www.xanthir.com/etc/railroad-diagrams/generator.html


^({(-?(?:0|[1-9]\d*)(?:\.\d+)?(?:[eE][+-]?\d+)?,|-?(?:0|[1-9]\d*)(?:\.\d+)?(?:[eE][+-]?\d+)?,-?(?:0|[1-9]\d*)(?:\.\d+)?(?:[eE][+-]?\d+)?|,-?(?:0|[1-9]\d*)(?:\.\d+)?(?:[eE][+-]?\d+)?)})*$

is JSOND a JSON superset? well it's a superset within the boundaries of json (to take advantage of the huge amount of json libraries)

http://www.json.org/fatfree.html
https://github.com/tabatkins/railroad-diagrams
http://www.xanthir.com/etc/railroad-diagrams/generator.html
https://www.debuggex.com/
http://en.wikipedia.org/wiki/Syntax_diagram
http://stackoverflow.com/questions/796824/tool-for-generating-railroad-diagram-used-on-json-org
http://www.ietf.org/rfc/rfc4627.txt?number=4627
http://docs.oracle.com/javase/tutorial/essential/regex/quant.html
http://www.regexplanet.com/advanced/java/index.html

JSOND takes a pragmatic approach to define JSON.
...

pseduocode:

validate:

if contents is not valid json fail
if contents root node is not an object fail
for each element in object or array
	if value is object or array recursive
	else if value is string
		if value starts with string
			definition = value - "string:"
			if definition exists but is not valid regexp fail
		else if value starts with number
			definition = value - "number:"
			if definition exists but does not match with regexp ^({(\d+,\d+|\d+,|,\d+)}|(\[|\()((-?(0|([1-9])\d*)(|\.\d+)(|(e|E)(\+||-)\d+)),(-?(0|([1-9])\d*)(|\.\d+)(|(e|E)(\+||-)\d+))|(-?(0|([1-9])\d*)(|\.\d+)(|(e|E)(\+||-)\d+)),|,(-?(0|([1-9])\d*)(|\.\d+)(|(e|E)(\+||-)\d+)))(\]|\)))+$ fail
		else if value does not start with with boolean fail
	else fail
	if key is empty fail
	else if key does not match with regexp § fail
success




contents is valid json
root node is an object
all values are objects, arrays or strings
all keys follow the following regular expression: ^(.+?)(:optional)?(#.+)?$
all string values follow the following regular expression:

^(string|number|boolean)$
^(string(:.+?)|number(:)|boolean)$

^{(\d+,\d+|\d+,|,\d+)}$

^{(\d,\d|\d,|,\d)}$

^:((\[|\()(\d,\d|\d,|,\d)(\]|\)))+$

# number value defintion
^(:({(\d,\d|\d,|,\d)}|(\[|\()(\d,\d|\d,|,\d)(\]|\)))+)?$
  // TODO replace \d with JSON number


^(({(\d+,\d+|\d+,|,\d+)})|(\[|\()((-?(0|([1-9])\d*)(|\.\d+)(|(e|E)(\+||-)\d+)),(-?(0|([1-9])\d*)(|\.\d+)(|(e|E)(\+||-)\d+))|(-?(0|([1-9])\d*)(|\.\d+)(|(e|E)(\+||-)\d+)),|,(-?(0|([1-9])\d*)(|\.\d+)(|(e|E)(\+||-)\d+)))(\]|\)))+$

(-?(0|([1-9])\d*)(|\.\d+)(|(e|E)(\+||-)\d+))

^/d$ #integer
^-?(0|([1-9])\d*)(|\.\d+)(|(e|E)(\+||-)\d+)$ #number

https://www.debuggex.com/


{
	"key#some key with a #name":"string:^#$"
}

#note that the comment is greedily taking all #
^(.*?)(:optional)?(#.+)?$


[{
    "id::The unique identifier for a product": "number",
    "name": "string",
    "price": "number:(0,]",
    "tags#JSOND cannot describe minimum number of items or unique items.": ["string:"],
    "dimensions:optional": {
        "length":"number",
        "width":"number",
        "height":"number"
    },
    "warehouseLocation:optional#Coordinates of the warehouse with the product": "geo"   
}]


[{
    "id::The unique identifier for a product": "number",
    "name": "string",
    "price": "number:(0,]",
    "tags:optional#JSOND cannot describe minimum number of items or unique items.": ["string:"],
    "dimensions:optional": {
        "length":"number",
        "width":"number",
        "height":"number"
    },
    "warehouseLocation:optional#Coordinates of the warehouse with the product": "geo"   
}]


[{
    "id::The unique identifier for a product": "number",
    "name": "string",
    "price": "number:(0,]",
    "tags:optional:JSOND cannot describe minimum number of items or unique items.": ["string#this is a comment (without the value definition part!)"],
    "dimensions:optional": {
        "length":"number",
        "width":"number",
        "height":"number"
    },
    "warehouseLocation:optional:Coordinates of the warehouse with the product": "geo"   
}]

[{
    "id::The unique identifier for a product": "number",
    "name": "string",
    "price": "number:(0,]",
    "tags:optional:#JSOND cannot describe minimum number of items or unique items.": ["string#this is a comment (without the value definition part!)"],
    "dimensions:optional": {
        "length":"number",
        "width":"number",
        "height":"number"
    },
    "warehouseLocation:optional:Coordinates of the warehouse with the product": "geo"   
}]


^(.+?)(:optional)?(#.+)?$


## TBD:

use reglar expression styled comments? 
(?#comment)

"string:#(?#comment)"
"number:{1,}#comment"
"boolean#comment"

vs

"string:#(?#comment)"
"number:{1,}(?#comment)"
"boolean(?#comment)"
"number(?#comment)"

vs

"string(?#comment)"
"number(?#comment)"


it might be a good idea to disallow comments for values!, there are a couple of reasons for this, string, number, boolean are values, as are {} and []. the biggest reason is that a regular expression is not controlled but the :optional field is! i.e. it's not allowed to write "key:##comment" while "value:##comment" could be a regular expression ##comment, or a regular expression # and a comment "comment".
This means that values in strings cannot be commented, the comment must be put on the array key, this is probably a reasonable restriction

in jsond you will have a problem if you try to have keys (or strings?) that contains :
thus for now ":" is not allowed anywhere except in string value defintions
you can choose your own separator á la sed? as long as you use the same, look at the ones after string,boolean,number

Look into doing more with naming of the "structure" and referencing that using file:// (default as relative to the current position in the file system) or http:// so that common structures can be published online and still linked properly, then start publishing a catalog of known structures. Also then look into publishing attributes/values as types? so that "http://jsond.org/definitions/email.jsond", "http://jsond.org/utc" or something similar can be used to not have to come up with our own regular expression for email or utcdate. "number", "string", "boolean" and {} [] are of course 'built-in'.

How to handle a jsond file that starts with an list? if it's possible in json is should be possible in jsond. thus a valid jsond file is: [], another is {} but then there cannot be any properties in that object, ["string"] is also allowed. the problem with the list is that they cannot be directly translated to an object, so how would you generate an object out of those? that problem is not within jsond domain but in jsond2java

How to publish jsond files that are not valid json, e.g. "date":"utc..." e.g. define own types

Publish this a a IETF draft if possible?

Link to page with examples or both regular expressions and numbers?

[1]: http://json.org
[2]: http://en.wikipedia.org/wiki/range_(mathematics)
[3]: http://en.wikipedia.org/wiki/Bracket
[4]: http://en.wikipedia.org/wiki/Integer
http://en.wikipedia.org/wiki/Regular_expression


xxx

- Definite brackets {} defines that any integer within the range are valid. E.g. {18,99} defines any integer between 18 and 99. Definite brackets are inclusive which means that 18 and 99 are valid while 17 and 100 are not.
- Closed brackets [] defines that any number within the range are valid. E.g. [18,99] defines any number between 18 and 99, including 18.00001 and 89.99999. Closed brackets are inclusive which means that 18, 18.0, 99, and 99.0 are all valid while 17.99999 and 99.00001 are not.
- Circle brackets () defines that any number within the range are valid. E.g. (18,99) defines any number between 18 and 99. Circle brackets are exclusive which means that 18.00001 and 89.99999 are valid numbers while 18 and 99 are not.


- Definite brackets {} are inclusive and defines any integer within the range.
	- {18,99} defines any integer that is equal to or higher than 18 and less than or equal to 99, such as 18 and 99, 	but not 17 or 100.
- Closed brackets [] are inclusive and defines any number within the range.
	- [18,98.5] defines any number that is equal to or higher than 18 and less than or equal to 98.5, such as 18, 18.00000, 18.00001, 98, 98.49999, and 98.5, but not 17.9999 or 98.500001.
- Circle brackets () are exclusive and defines any number within the range.
	- (18,99) defines any number that is higher than 18 and less than 99, such as 18.00001 and 89.99999, but not 18 or 99.

#### Examples

Expression | Valid values | Values
----|------|----
{18,99} | 18, 19, 98, 99   | 17, 100
bar | bar  | bar
bar | bar  | bar
baz | baz  | baz

superlong list of examples which are all proper jsond
maybe not written in markdown but actual checked in files, that can be modified using the tool?
also link to the actual files from an markdown examples page?

Closed and circle brackets can be combined, e.g. (18,99] can be used to define any number that is higher than 18 and less than or equal to 99. Definite brackets cannot be combined with closed or circle brackets. ranges are themselves separated with a comma, e.g. {18,24},{26,99} defines any integer between 18 and 99, including 18 and 99, but not 25. 

Any type of range can be combined, e.g. [18,19),{19,99} defines any number that is larger than or equal to 18 but less than 19, and any integer between 19 and 99 including 19 and 99. Either the lower or higher number can be omitted to define an open range, e.g. {0,} defines any positive integer, including 0, and {,-1} defines any negative number, not including zero.


"hobbies": . It is possible to define more than one type by separating them with a comma, e.g. "hobbies": ["string","number"] defines that a hobby is either a string or a number.

