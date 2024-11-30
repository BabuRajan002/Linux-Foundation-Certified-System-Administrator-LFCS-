# Regular Expressions

## `^` - Indicates begin with 

**Example** 

grep '^#' /etc/login.defs

* Extracts the lines which are starting with '#' sign

`grep '^PASS' /etc/login.defs`

* Extracts the lines which are starting with 'PASS' string

## `$` - Indicates the line ends with 

**Example** 

`grep -w '7$' /etc/login.defs`

* Extracts the lines which are sending with '7' 

## `.` Match any ONE character

**Example** 

`grep -r 'c.t' /etc/`

* Extracts a word  which is having only ONE character between c and t. (Example: execute, cat..etc)

`grep -wr 'c.t' /etc/`

* Using the w option we are only extracting the word. 

## `*:` Match the previous element 0 or More times

**Example** 

`grep 'let**' /etc/`

* It matches a word starting 'let' as many characters after it. 

`grep -r '/.*/' /etc/` 

* Begins with /; has 0 or more characters between ; ends with a /





