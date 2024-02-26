>before starting with regex always figure out whether the regex is basic, extended or perl

# Tools

[regex101](https://regex101.com/)

# Basics

anchors
```bash
^The       # matches any string that starts with The
end$       # matches a string that ends with end
^The end$  # exact string match (starts and ends with The end)
roar       # matches any string that has the text roar in it
```

quantifiers
```bash
abc*       # matches a string that has ab followed by zero or more c
abc+       # matches a string that has ab followed by one or more c
abc?       # matches a string that has ab followed by zero or one c
abc{2}     # matches a string that has ab followed by 2 c
abc{2,}    # matches a string that has ab followed by 2 or more c
abc{2,5}   # matches a string that has ab followed by 2 up to 5 c
a(bc)*     # matches a string that has a followed by zero or more copies of the sequence bc
a(bc){2,5} # matches a string that has a followed by 2 up to 5 copies of the sequence bc
```

OR operator
```bash
a(b|c)     # matches a string that has a followed by b or c (and captures b or c)
a[bc]      # same as previous, but without capturing b or c
```

character classes
```bash
\d        # matches a single character that is a digit
\w        # matches a word character (alphanumeric character plus underscore)
\s        # matches a whitespace character (includes tabs and line breaks)
.         # matches any character
\D        # matches a single non-digit character
\$\d      # matches a string that has a $ before one digit
```

flags
```bash
/g       # global
/m       # multi-line
/i       # case insensitive
```

# Intermediate

grouping / capture groups
```bash
a(bc)           # parentheses create a capturing group with value bc 
a(?:bc)*        # using ?: we disable the capturing group
a(?<foo>bc)     # using ?<foo> we put a name to the group
```

bracket expression
```bash
[abc]           # matches a string that has either an a or a b or a c
[a-c]           # same as previous
[a-fA-F0-9]     # a string that represents a single hexadecimal digit, case insensitively
[0-9]%          # a string that has a character from 0 to 9 before a % sign
[^a-zA-Z]       # a string that has not a letter from a to z or from A to Z.
```

greedy vs lazy match
```bash
<.+?>           # matches any character one or more times included inside < and >, expanding as needed
<[^<>]+>        # matches any character except < or > one or more times included inside < and >
```

# Advanced

boundaries
```bash
\babc\b         # performs a "whole words only" search
\Babc\B         # matches only if the pattern is fully surrounded by word characters
```

back references
```bash
(abc)\1             # using \1 it matches the same text that was matched by the first capturing group
(abc)(de)\2\1     # we can use \2 (\3, \4, etc.) to identify the same text that was matched by the second (third, fourth, etc.) capturing group
(?<foo>[abc])\k<foo>  # we put the name foo to the group and we reference it later (\k<foo>). The result is the same of the first regex 
```

look-ahead and look-behind
```bash
d(?=r)      # matches a d only if is followed by r, but r will not be part of the overall regex match
(?<=r)d     # matches a d only if is preceded by an r, but r will not be part of the overall regex match
d(?!r)      # matches a d only if is not followed by r, but r will not be part of the overall regex match
(?<!r)d     # matches a d only if is not preceded by an r, but r will not be part of the overall regex match
```