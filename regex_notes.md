# Three Basic Types

- basic
- extended
- perl

# Basic vs Extended
- basic use \ prior to grouping/capturing parens, extended don't
- basic use \ before + to mean "1 or more of the previous char groups", extended do not
- basic use \ before ? to mean "0 or 1 of the previous char group", extended do not

# Perl vs Extended
- Perl optionally uses $ instead of \ to represent vars in substitution patterns
- Perl supports non-capturing parens
- The order of multiple options within parens is important when using substrings
- Perl permits literal [] anywhere within a char class when escaped
- Perl adds a number of additional switches that are equivalent to certain chars and classes
- Perl supports a broader range of modifiers

# Syntax
- /search_pattern/modifiers
- command/search_pattern/modifiers
- command/search_pattern/replacement/modifiers

- pattern 1 returns the lines matching. grep requires the slashes to be ommitted
- second syntax is used for most commands
- third is used for search and replace commands
- availability of commands and flags depends on the tool/regex variant

# Positional Anchors
^ - when placed at the beginning matches a pattern at the beginning of a line of text
$ - when placed at the end matches an pattern at the end of a line text

# Wildcards and Repetition Operators
wildcard - symbol that takes the place of any other symbol
. - any symbol
* - matches 0 or more of the preceding token, matches as many as possible vefore mocing onto the next token
+ - in basic regular expressions, the + operator must be preceded by \ (i.e. \+, which in other cases matches the literal + character)
this operator matches one or more of the preceding token
? - matches 0 or 1 of the preceding token, i.e makes the preceding token optional (also requires \ in basic regexes)

# Character Classes and Groups
- POSIX character classes and groups
   - [:alnum:] - all alphanumeric chars
   - [:alpha:] - all alphabetic chars
   - [:blank:] - aall whitespace within a line
   - [:cntrl:] - all control characters (ASCII 0 - 31)
   - [:digit:] - all numbers
   - [:graph:] — all alphanumeric or punctuation characters.
   - [:lower:] — all lowercase letters (a-z).
   - [:print:] — all printable characters (opposite of [:cntrl:], same as the union of [:graph:] and [:space:]).
   - [:punct:] — all punctuation characters
   - [:space:] — all whitespace characters (space, tab, newline, carriage return, form feed, and vertical tab). (See note below about compatibility.)
   - [:upper:] — all uppercase letters.
   - [:xdigit:] — all hexadecimal digits (0-9, a-f, A-F).

- grep does not accept [:space:] because it includes line breaks and grep works on lines
- sed accepts [:space:] but treats it like [:blank:]

# Custom Character Classes
- combine ranges or specific selection groups for custom matching (i.e. [a-zA-Z ] etc)
- use ^ at the beginning to specify not

# Grouping Operators
- Grouping syntax also results in a capture
- sed, awk, and grep use basic regexes, and so () for groups must be escaped using \
- to use extended regexes with these tools use the -E flag, except with sed, which you should use basic regexes
- the | operator within a group results in or or optional matches
- for optional groups, use ()? or (match()) (i.e. an empty group within a group)

# Quoting Special Characters
- When using custom character classes within Perl, you can place it anywhere. In basic or regular extended,
to make a clsoe bracket be a memeber of a custom char class, you make it the first char in the class

# Capturing Operators and Variables
- with basic, within the replacement segment, any capture groups can be represented with \1,\2, etc.
- with Perl, you can also reference them with $1, $2, etc.

# Modifiers
- g - replace globally (defaults to just matching first)
- i - use case insensitive matching (grep -i)
- m - multiline matching
- o - compile once (Perl extension) - In perl, vars must be recompiled everytime, use this if you know
the var will not change
- s - single line matching
- x - extend readability, allows you to write them across multiple lines

# Extended Regexes in Perl, Python, JavaScript
- non-greedy wildcard matching
- noncapturing parens
- \A—anchors matching to the beginning of the string as a whole (but not the beginning of lines within the string).
- This shortcut is not broadly supported outside of Perl. In other languages, use ^ and add the /s modifier (or do not specify the /m modifier, depending) to specify line-at-once matching.

- \b—word boundary (see note).
- \B—nonword boundary (see note).
- \d—equivalent to [:digit:].
- \D—equivalent to [^:digit:].
- \f—form feed.
- \n—newline.
- \p—character matching a Unicode character property that follows. For example, \p{L} matches a Unicode letter.
- \P—character not matching a Unicode property that follows. For example, \P{L} matches any Unicode character that is not a letter.
- \r—carriage return.
- \s—equivalent to [:space:].
- \S—equivalent to [^:space:].
- \t—tab.
- \u—a single Unicode character in JavaScript regular expressions. This shortcut must be followed by four hexadecimal digits.
- \v—vertical tab.
- \w—equivalent to [:word:].
- \W—equivalent to [^:word:].
- \x—start of an ASCII character code (in hex). For example, \x20 is a space.
- \X—a single Unicode character (not supported universally). This shortcut must be followed by four hexadecimal digits.
- \z—anchors matching to the end of the string as a whole (but not the end of lines within the string).
- This shortcut is not broadly supported outside of Perl. In other languages, use $ and add the /s modifier (or do not specify the /m modifier, depending) to specify line-at-once matching.

- \Z—anchors matching to the end of the string as a whole (but not the end of lines within the string). In some languages (including Perl), this matches prior to the closing line break if the string ends with a line break. To avoid this, use \z instead.
- This shortcut is not broadly supported outside of Perl. In other languages, use $ and add the /s modifier to specify line-at-once matching.
