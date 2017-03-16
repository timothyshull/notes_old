# awk
awk is a pseudo-C interpreter that can be used for easier filter and reporting (like ack or ag but more comprehensive)
awk is designed around input text into records and the records being broken into fields
After the awk interpreter reads a record in, divides the record into fields dependent on the set field separator
The awk script specifies rules, and the rules operate on fields and records and output them

# Basics
- syntax similar to C
- interpreted, so not as fast as C
- final semicolons are optional
- newlines end the statement, but \ allows it to continue to the next line
- a script does not have a main function. it is divided into a set of filter actions surrounded by curly braces.
the filters are applied sequentially for each record in an input.
- variables are in the global scope except for parameters within a function
- variables are set (essentially immutable) until explicitly cleared
- variables are not preceded by $ except for $0 - entire record, and $1 etc for fields within a record. No variable interpolation like in shel scripts.
- the awk interpreter always requires an input file even if nothing is done with the input
- the regular expression used for actions is placed before the filter action

# Basic structure
pattern { action }
the pattern is a test that is run with each line read as input

BEGIN ... END - actions taken before any lines are read adn after last line is read

awk recognizes normal escapes where bash etc do not
variables are just normally assigned variables, $x refers to the fifth field or column on the input line

make awk files executable with #!/bin/awk -f
(/usr/local/bin/awk -f on my system)

# Expressions
awk supports arithmetic expressions as expected

# Regular Expressions
~ - matches
!~ - not matching

# List of awk commands syntax

if ( conditional ) statement [ else statement ]

while ( conditional ) statement

break
continue
{ [ statement ] ...}

variable = expression
print [expression-list ] [ > expression]
printf format [ , expression-list ] [ > expression ]
next
exit

# awk variables
$0 - an entire record read in an awk script, i.e. an entire line
$1 - the first field on a line, i.e. full word
positional variables - function triggered by the dollar sign with integer position

# FS
FS - the field separator variable. awk parses files and separates fields based on this variable.
A record is separated by \n and fields are separated by whatever is specified in the FS variable.
This variable is generally whitespace, a regular expression, : , ; . If left null, the line is
split into one filed per character. The FS specifier can be changed multiple times within a single
script.

# OFS
The output field separator determines how fileds are combined when output.
print $2 $3 - just concatenates the two fields together
print $2, $3 - outputs two separate fields, separated by the OFS

# NFS
Variable to represent the number of fields on a line

# NR
The number of records variable (i.e. number of lines) or line number

# RS
The record separator variable. Like the file system variable but for line separations

# ORS
The output record separator variable. Similar to OFS for lines.

# Additional
ARGC, ARGV, ARGIND, FNR, OFMT, RSTART, RLENGTH, SUBSEP, ENVIRON, IGNORECASE, CONVFMT, ERRNO, FIELDWIDTHS

# Associative arrays
Awk uses associative arrays - fill in later

# FILENAME
The current filename

# awk command utilities
awk scripts converted to command utilities take a single "-f" argument after the #! path. When run
as an executable, the script cannot be passed arguments. When run with the awk command, the FS variable
can be specified with the -F option.

# print
awk supports perl-like print and c-like printf

# File output
> writes to file
>> appends to file

# Awk Functions


USE GAWK

# Use getline
getline < "secondary.input" to read text from a file

# Output to file
- all output to single file - call script with > filename.txt
- output to specific files in script
BEGIN {
        print "This is a test." | "/usr/bin/tail -n 1";
        print "This is only a test." | "/usr/bin/tail -n 1";
        close("/usr/bin/tail -n 1");
        print "Yikes!" | "/usr/bin/tail -n 1";
 
        print "This is another test" > "/tmp/testfile-awk"
        print "This is yet another test entirely" > "/tmp/testfile-awk"
}

# Read from file
BEGIN {
        getline < "/tmp/testfile-awk";
        print "The record was " $0;
 
        "/bin/echo 'This is a test line'" | getline
        print "The second record was " $0;
}

# Read from file as program file
- from command line
awk -f awk_script_.awk infile.txt


# BEGIN & {} & END
BEGIN {} & END {} - executes as script
    MIDDLE {} - executes as loop through all lines in infileh12gY


