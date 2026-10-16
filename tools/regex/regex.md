# Regex

Regex is used for searching text for specific patterns of characters and strings. This will provide a reference guide as well as examples on how to use each of the main regex concepts.


# Cheat Sheet

### Characters
| Pattern | Matches | Example | Hits |
|---|---|---|---|
| `abc` | the literal text `abc` | `/cat/` | `cat` |
| `.` | any character except a newline | `/c.t/` | `cat, cot, c9t` |
| `[abc]` | one of `a`, `b` or `c` | `/b[aiu]r/` | `bar, bir, bur` |
| `[^abc]` | any character **except** `a`, `b`, `c` | `/b[^eo]r/` | `bar, bur` (not `ber, bor`) |
| `[a-z]` | one letter in the range | `/[e-o]/` | `e, f, ... o` |
| `[0-9]` | one digit in the range | `/[3-6]/` | `3, 4, 5, 6` |
| `\` | escapes a special character | `/\./` | a literal `.` |

### Shorthand classes
| Pattern | Matches | Opposite |
|---|---|---|
| `\w` | letter, digit or underscore (`[A-Za-z0-9_]`) | `\W` |
| `\d` | digit (`[0-9]`) | `\D` |
| `\s` | whitespace: space, tab, line break | `\S` |

### Repetition
| Pattern | Meaning | Example | Hits |
|---|---|---|---|
| `*` | 0 or more | `/be*r/` | `br, ber, beer` |
| `+` | 1 or more | `/be+r/` | `ber, beer` |
| `?` | 0 or 1 (optional) | `/colou?r/` | `color, colour` |
| `{n}` | exactly *n* | `/be{2}r/` | `beer` |
| `{n,}` | *n* or more | `/be{2,}r/` | `beer, beeer` |
| `{x,y}` | between *x* and *y* | `/be{1,3}r/` | `ber, beer, beeer` |
| `*? +? ??` | lazy: as **few** as possible | `/<.+?>/` | `<b>` (not `<b>bold</b>`) |

### Anchors
| Pattern | Matches |
|---|---|
| `^` | start of the line (start of the text without `m`) |
| `$` | end of the line (end of the text without `m`) |

### Groups and alternation
| Pattern | Meaning | Example | Hits |
|---|---|---|---|
| `(abc)` | capturing group | `/(ha)+/` | `ha, haha` |
| `\1` | the text group 1 captured | `/(ha)-\1/` | `ha-ha` |
| `(?:abc)` | group without capturing | `/(?:ha)+/` | `haha` (nothing stored) |
| `a\|b` | `a` or `b` | `/(c\|r)at\|dog/` | `cat, rat, dog` |

### Look-arounds
| Pattern | Meaning | Example | Hits |
|---|---|---|---|
| `X(?=Y)` | `X` followed by `Y` | `/\d+(?=PM)/` | `3` in `3PM` |
| `X(?!Y)` | `X` **not** followed by `Y` | `/\d+(?!\d\|PM)/` | `3` in `3AM` |
| `(?<=Y)X` | `X` preceded by `Y` | `/(?<=\$)\d+/` | `5` in `$5` |
| `(?<!Y)X` | `X` **not** preceded by `Y` | `/(?<![$\d])\d+/` | `5` in `5 apples` |

### Flags
| Flag | Name | Effect |
|---|---|---|
| `g` | global | find every match, not just the first |
| `m` | multiline | `^` and `$` match at each line instead of the whole text |
| `i` | case-insensitive | `A` and `a` are treated the same |


# Reference


## Core concepts

### Matchers
If we want to simply find the word in the text we must type the `<word>` into the regex expression.

### Dot `.`
Select any character using the `.`, which matches any single character except a line break.

### Character Sets `[abc]`
If a character in a word can be more than one of several characters, (e.g. 'bar', 'bir', 'bur'), we can look for the character through:
`/b[aiu]r/g` and regex will find all words with that match

### Negated Character Sets `[^abc]`
We can also detect all words which *do not* contain certain characters:
Ex:
`/b[^eo]r/g` --> matches `bar, bur` but not `ber, bor`

### Letter Range `[a-z]`
To search for a specific letter range, we can use:
`[a-z]`
Ex:
`/[e-o]/g`

### Number Range `[0-9]`
To find a range of numbers in a string we can write the first and last digits inside square brackets `[]` with a `-`. This includes the first and last digit which was searched

## Repetitions
Special characters `+, *, ?` are used to specify how many times a character can repeat or whether it appears at all.

### Asterisk `*`
The Asterisk is used when a character may either not match at all **or** can match many times

Ex: `/be*r/g`  gathers all `br, ber, beer`

### Plus Sign `+`
The plus sign `+` is used when a character **must** occur one or more times, and it is put after the character referenced

Ex: `/be+r/g` gathers `ber, beer` but not `br`

### Question Mark `?`
To indicate that a character is **optional**, we put a `?` question mark after that character

Ex: `/colou?r/g` gathers `color, colour`

### Curly Braces `{}`
To require a character to appear an exact **number of times**, we use `{n}` after the character referenced

To require a character to appear **at least** *n* times, we use `{n,}`

When we want a character to occur between a specified range of occurrences we use `{x,y}`, where `x` is the minimum amount of times it will occur and `y` is the max times it will occur.

Note: don't put a space after the comma. In most engines `{2, 4}` is not a quantifier and is matched as literal text.

## Grouping

### Parentheses `()`
This groups an expression together so it can be referenced from other expressions

### Referencing a Group
When referencing a group, we use `\` followed by the group's number, `\1` for the first group, `\2` for the second, etc.

Ex. `/(ha)-\1,(haa)-\2/g` --> matches `ha-ha,haa-haa`

### Parentheses `(?: )`: Non-capturing Grouping
Adding the `?:` after an open parentheses makes a group **non-capturing**, which groups the expression, but the match is not stored, so it cannot be referenced

### Pipe Character `|`
All possible expressions are separated by the pipe sign `|`
Ex.
`/(c|r)at|dog/g` --> `cat, rat, dog`

### Escape Character `\`
To match the special characters, we need the escape character `\` before them.

Ex. `/\./g` matches a literal `.`, and `/\(\d+\)/g` matches `(42)`

### Caret Sign `^`: Selecting by Line Start
This selects the characters at the **beginning** of a line

### Dollar Sign `$`: Selecting by End of Line
Find the character/word at the **end** of the line

### Word Character `\w`: Letter, Number, and Underscore
This expression `\w` is used to find **letters, numbers, and underscore** characters. Similar to `.`, but does not find `.,:,!,?` or spaces

### Non-Word Characters `\W`
Used to find other characters that **are not** letters, numbers, and underscores

### Digit Character `\d`
Finds only digits

### Non-Digit Characters `\D`
Used to find any character that is not a digit

### White space Character `\s`
Used to find **only** whitespace characters, such as **spaces, tabs, line breaks**

### Non-white space Characters `\S`
Used to find any character that is **not whitespace**

## Look Arounds
Use look-arounds when the expression is supposed to run only when it comes before or after another expression. The look-around itself is not part of the match.

### Positive Lookahead: `(?=)`
Ex. select only the numbers followed by PM:
`/\d+(?=PM)/g`
So, this first looks for one or more digits, then checks that `PM` comes right after them. `PM` is checked but not included in the match.

### Negative Lookahead `(?!)`
Select numbers that are not followed by a specific character:

Ex. Select numbers not followed by PM:
`/\d+(?!\d|PM)/g`

Note: `/\d+(?!PM)/g` alone is not enough. In `10PM` regex backs off to just `1`, which is followed by `0` (not `PM`), so `1` matches. Adding `\d` to the lookahead stops it from matching part of a number.

### Positive Lookbehind: `(?<=)`
If we want to get the number or text behind a character:
Ex. select the price value in the text:
`/(?<=\$)\d+/g`

### Negative Lookbehind: `(?<!)`
Select only numbers not preceded by `$`, we write `(?<!)` before the expression

Ex. `/(?<![$\d])\d+/g` (the `\d` stops it from matching the `0` in `$50`)

Note: lookbehinds are not supported everywhere, e.g. `grep -E` and `sed` don't have them. Use `grep -P` instead.

## Flags

### Global Flag `g`
Makes the expression select all matches. If not used, only the first match is selected

### Multiline Flag `m`
Makes `^` and `$` match at the start and end of each line, instead of treating the whole text as a single line

### Case-insensitive flag `i`
Ignores the differences between uppercase and lowercase letters

### Greedy matching
By default, repetitions (`*, +, ?, {}`) are greedy: they match **as many** characters as possible.

Ex. `/<.+>/` on `<b>bold</b>` matches the whole string

### Lazy matching
Adding a `?` after a repetition makes it lazy: it matches **as few** characters as possible.

Ex. `/<.+?>/g` on `<b>bold</b>` matches `<b>` and `</b>` separately


## Examples


## References
