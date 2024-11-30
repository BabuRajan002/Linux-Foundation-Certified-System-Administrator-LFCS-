# Extended regular expressions

* When have a confusion about special characters whether to use \ or not we can use the extended regular expressions

`grep -Er` or we can use `egrep` command

{}: To specify previous elements can exist "this many" times

**Example** 

`grep -Er '0+' /etc` or `egrep -r '0+' /etc/` 

* Extracts the line with digit Zero one or more times. 

`egrep -r '0{3,}' /etc`

* {minRepetitions, maximumRepetitions} - Here 3 indicates minimum number of repetitions. Blank Value indicated maximum number of repetitions.

`egrep -r '0{3} /etc/'` 

* Extracts exactly 3 repetitions in this case.

## ?: Make the previous element optional

`egrep -r 'disabled?' /etc/` 

* Here it makes 'd' is optional. So it extracts disable or disabled 

`egrep -wr 'disabled' /etc/` 

* Using w option we can extract the exact match

## |: Match one thing or the other

`egrep -r 'enabled|disabled' /etc/`

* Extracts either match for enabled or disabled

## []: Ranges or Sets

`egrep -r 'c[ua]t'` /etc/

* Extracts any matches either between a or u in between c and t. For example cut, cat

## [^]: Negated Ranges or Sets

`egrep -r 'https[^:]'` /etc/

* Extracts https and which is not a URL. Above expression ignores the part after the semicolon (:)

***Important:***
Refer wbesites like Red xr.com to practice



