
### 2. JSOND Grammar

A JSOND structure is also a JSON structure. In addition to JSON Grammar [RFC4627], JSOND adds a few formatting rules.

The use of structural characters as described in this document is within a JSON name or value field. The structural characters are also used in JSON where they have a different meaning.

These are the eight structural characters:

	name-separator  = ws %x3A ws  ; : colon
	value-separator = ws %x2C ws  ; , comma
	begin-array     = ws %x5B ws  ; [ left square bracket
	begin-object    = ws %x7B ws  ; { left curly bracket
	begin-array     = ws %x5B ws  ; ( left circle bracket
	end-array       = ws %x5D ws  ; ] right square bracket
	end-object      = ws %x7D ws  ; } right curly bracket
	end-array       = ws %x5D ws  ; ) right circle bracket

	name-separator		= ws %x3A ws  ; : colon
	value-separator	     = ws %x2C ws  ; , comma
	inclusive-integer-interval-start    = ws %x7B ws  ; { left curly bracket
	inclusive-integer-interval-end	    = ws %x7D ws  ; } right curly bracket
	inclusive-number-interval-start     = ws %x5B ws  ; [ left square bracket
	inclusive-number-interval-end       = ws %x5D ws  ; ] right square bracket
	exclusive-number-interval-start     = ws %x28 ws  ; ( right square bracket
	exclusive-number-interval-end       = ws %x29 ws  ; ) right curly bracket

No whitespace is allowed before or after any of the structural
characters.

#### 2.1. Names

A JSOND name MUST be a JSON string and MUST contain at least one character.

	name = string

JSOND does not define an upper bound for the number of characters in a name.

The string MAY end with the name-separator. The name-separator MAY be followed by one of the following modifiers:

	null undefined

The undefined modifier MUST be interpreted as that the name/value pair is optional and MAY not be present in the JSON structure.

The null modifier MUST be interpreted as that the value in the name/value pair may be null.

#### 2.2. Values

A JSOND value MUST be a JSON object, array, or string.

	value = object / array / string

If the value is a string the string MUST start with one of the following three literal names:

	type = string / number / boolean

The types MUST be lowercase. No other literal names are allowed.

#### 2.3. Strings

If the value is defined as a string, regular expressions [POSIX.2] MAY be used to define the string.

The literal name string and the regular expression is separated with the name-separator.

	string name-separator regular-expression

#### 2.4. Numbers

If the JSOND value is defined as a number, an arbitrary number of mathematical intervals [REF] MAY be used to define the number.

The literal name number and the intervals is separated with the name-separator.

	number name-separator [ interval *( interval ) ]

An interval is either an integer interval or a number interval.

	interval = integer-interval / number-interval

##### 2.5.1 Integer Interval

A integer interval is used to denote that the number is a range of integers.

	integer-interval = begin-integer-interval integer value-separator integer end-integer-interval

An integer is defined as any number without decimals.

	digit	 =  %x30-39
	integer     =  [ digit *( digit ) ]

   // TODO: support JSON style of e and stuff?
   // or to support any number but be floored and only inclusive integers are counted

##### 2.5.2 Number Interval

A number interval is used to denote a range of JSON numbers.

	   number-interval = inclusive-begin-interval / exclusive-begin-interval number value-separator number inclusive-end-interval / exclusive-end-interval

#### 2.6. Booleans

The boolean type is used to denote that the JSON value MUST be of one the JSON literal names:

	boolean = true / false
