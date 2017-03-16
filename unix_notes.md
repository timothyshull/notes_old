# IO Sources
stdin - file descriptor that is the means through which a program obtains input from user or other processes
stdout - output file descriptor
stderr - output stream for error messages, debug messages, or other non-standard output

# Current Directory
. or ./

# Home Dir
~
$HOME - also represents home directory

# Change Dir
cd - navigate
+ .. - navigate up
cd (blank) - navigate home

# Present Working Directory
pwd
pwd -P - physical path with symlinks resolved

# List Files/Dirs
ls
-a - all
-l - long, prints permissions, group, user, size, date, location of symlinks, etc
-h - prints sizes with human readable formats
-S - sort by size
-t - sort by last modified time
-r - reverse sort

# Links
ln - creates a hard link between two files, i.e. creates another file on disk that is updated each time the original is updated, you cannot create a hard link between two files on different systems
-f - force link
-s - symbolic link - used for files or dirs (unlike hard links), can link to files on other systems, actually only creates a file pointer rather than replicating the file

# Make Dir
mkdir - create a directory
-p - also create intermediate directories
-v - verbose output

# Copy Files
cp <file1> <newFile> - make a copy of file1 named new file
cp <file1> <file2> <dir> - copy file1 & 2 into dir (can do any number of files but last arg must be a dir
cp <\*.ext> <dir> - copy all files of type ext into dir
-v - verbose
-R - copy a directory recursively
-f - force file overwrite
-i - confirm overwrite if it is about to occur

# Delete files
rm - delete files or folders

# Move files
mv - move, supports same flags as cp (combines cp and rm)

# IO
| - aka pipe, redirects stdout or stderr to the following command
> - writes to a file
< - read from a file

# ag
ag - search for text, first arg is search term, second arg is path
supports a limited subset of regular expressions, defaults to regexes
-Q - queries by literal expression instead of regex
-l - just list file names where search was found without line number and context
-i - case insensitive
-G - scope to filenames (takes an arg that is also a regex)
-ignore-dir - ignore a path (also allows filtering of files, i.e. --ignore-dir="\*.rb")
ag, unlike ack, reads VCS files like gitignore and filters based on their directives
--skip-vcs-ignores - do not filter files listed in .gitignore, etc
.agignore - file that you can use like a gitignore for ag
.ackrc - like agignore for ack
ag and ack accept stdin as text to search over (i.e. through a pipe command)
ag is faster

# curl
curl - make http request to a given url
-i - information
-L - follow intermediate redirects
-O - download a file
-o - specify a new name for a file download
-X - change http request method
?param=etc - attach query string to url
-d - pass data in request body, i.e. attach a query string (-d "param1=val1&param2=val2"), can also pass JSON (-d "{\"param\":\"val\"}"), or pass a file as a request body (-d @filename)
-F - pass form params with a post request (-F param=val, or nested -F param[subparam1]=val1 -F param[suparam2]=val2), or upload a file (-F photo=@filename)
-H - set headers, (-H "Accept: application/json")
-u - user authorization (-u "user1:password1", used in conjunction with a POST)
-D - dump headers and cookies into a specified file, this is done to retrieve session cookies on subsequent requests
-c - save just the cookies to a file
-b - send the contents of a cookie file to the server with a request

# Find
find - just search for files whose path or name matcha query term
formula - find <path to search> <options> <pattern> (pattern must be prefixed with a \ when looking at regexes that start with special characters)
-name - exact match
-path - search for files whose path contains the search expression
-type - -f or -d find only files or directories
-or - or expressions - can create multi-part queries that are combined by wrapping in \( query1 -or query2 \)
-not - not matching (equivalent to \!)
-mtime - find by last modified time (find . -mtime -l - find files that have been modified in the last day)
-mmin - find files that have been modified in the last n minutes ( find . -mmin -5)
-size - find by size (-size +200K)
-delete - delete matched files
-print - print result

# Grep
grep - similar to ack or ag but is found on almost all Unix/Linux systems, just slower
formula - grep <search term> <location>
search terms default to regexes
-c - count occurences
-n - display line numbers
-i - case insensitive
search multiple files - pass multiple files after the search term, or use wildcards \*.rb etc
-R - search recursively through directories
--include - limits scope of files in -R search to filenames that match pattern
- pipe from standard input - cmd | grep <search>
-v -  invert searches

# Ps
ps - list active processes
PID - process id, TTY - the controlling terminal for the process (teletype), TIME - time since the process started, CMD - name of the command that is running
u - display user related information (i.e. which user initiated the process)
USER - user running process, %CPU - percent of time the process has used the CPU since it started, %MEM - percent of real mem the process is using, VSZ - virtual memory size of the process, RSS - real memory size of the process, STAT - status of the process, STARTED - when the process was started
-e - display all processes
ax - similar to e but different display
-U <user> - display processes by user
-L - list possible columns to display
-O - pass comma separated list of desired columns to display
-m - sort by memory usage
-r - sort by CPU usage
a - show processes for all users
u - display the process' user/owner
x - also show processes not attached to a terminal

# Sed - stream editor
sed - edit files or streams (stdin, stdout, stderr, etc) using pattern matching and replacements
 defaults to sending changes to stdout
formula - sed "s/regex/replacement" <file to output to> - wrap first arg in quotes to escape
this will default to only replacing the first match in each line
"s/regex/replacement/g" - this specifies global replacement
nth replacement - "s/regex/replacement/n" replace nth occurence of regex match on each line with replacement
-i - edit file in place
-i.tmp - copy original file to filename.ext.tmp to backup
"s/regex/replacement/i" - case-insensitive searches
"/regex/s/regex/replacement/g" - find lines that contain regex 1 and replace every character matching regex 2 in that line with the replacement
& - when used in the replace section fo the script, it represents the matched pattern
\u& - uppercase the matched pattern
\l& - lowercase pattern matching
-n - suppress or control output
-n + "s/regex/replacement/p" - print only modified content
-n + "s/regex/replacement/pw file" - print only modified content and write to file
-n '1~2p' - print only every other line of the file
"/regex/d" - delete lines matching patter
multiple sed commands - either use the | operator or use 's/regex/replacement/<options>;<sed cmd 2>' etc

# Tar
tar - create a tape archive - difference between ar - tar maintains directory structure while ar contains a flat directory structure
-c - create a new archive, just prints to stdout
-cf - save to file
-v - verbose
mult directories or files - tar -cvf out.tar dir1/ dir2/ file1 file2 etc
find piped to tar - find <dir> <options> <search> | tar -cvf <out.tar> -T -
-t - list files in an exisiting archive
-r - append files to exisiting archive, not compatible with compressed archives
-u - update a file
-x - extract form the archive
-x - single file - tar -xvf <archive.tar> <file>
-C - extract to a different folder
-z - compress with gzip, -j compress with bzip, -Z compress with Unix compress
-xvzf - extract comrpessed files

# Cal
cal -print calendars

# Cat
cat - without any flags, cat will print the contents of a file to stdout, otherwise concatenates files or streams to files together
cat <fle1> <file2>  - puts two file stogehter and streams to stdout
cat <file1> <file2> > <new_file>
-b - display line numbers
cat by default does not do much but can be used to append large amounts of text to files (i.e.
cat > <file> << HEREDOC
...text...
HEREDOC

# Kill
kill - send messages to a process
1 - SIGHUP - Term - Hangup on controlling terminal death of controlling process
2 - SIGINT - Term - Interrupt from keybd
3 - SIGQUIT - Core - Quit from keybd
4 - SGILL - Core - Illegal instruction
6 - SIGABRT - Core - Abort signal from abort(3)
8 - SIGFPE - Core - floating point exception
9 - SIGKILL - Term - Kill signal
11 - SIGSEGV - Core - Invalid memory reference
15 - SIGTERM - Term - Termination signal

kill defaults to 15
kill -9 PID - force kill
kill -s SIGKILL PID - force kill

# Man
man - manual for cmd

# Pbcopy
pbcopy - only available on mac, reads data from stdout and copies into system pastebuffer

# Pbpaste
pbpaste - also mac only, takes data from system pastebuffer and writes to stdout

# Tail
tail - allows viewing of the end of a file including watching during changes

# Tree
tree - list directory contents in tree-like format
-L - limit depth
-d - list directories only

# Wc
wc - count words in a file
wc -w - count lines in a file

# Echo
echo - echoes a string to stdout, i.e. repeats the result but includes any additional processing like substitution, etc

# printf
printf - like echo but does not automatically print a newline (also like C printf)

# read
read input from stdin - read <var> <var> etc - defaults to stopping when a newline is encountered, can modify IFS

# chmod
chmod <options> ... Mode ... file
chmod <options> ... Numeric_Mode file...
chmode <options> ... --reference=RFile file...

-f suppress errors
-v verbose
-c changes - report when changes are made
-R change fiels and directories recursively

permissions grid - (-rwxrwxrwx) refers to read, write, executable for owner, group, other
numeric mode -

symbolic mode - [ugoa][+-=][rwxXstugo]
u - user who owns it
g - other users in the file's group
o - other users not in the file' group
a - all users

+ - add specified permissions to files
- - remove specified permissions from files
= - set specified permissions as only permissions

r - read
w - write
x - execute (run)
X - execute only if file is a dir
s - set user or group ID on execution
t - save program text on swap device
u - permissions that user owner of file currently possesses
g - permissions that group has
o - permissions that users outside of group have

# chgrp
chgrp - same options as chmod but changes group ownership to match the same group as an existing reference file

# chown
chown - change user and group ownership of a file
chow <options> ... new-owner file
chown < options> ... :Group file
chown <options> ... --reference-RFILE file

# stat
stat - display status of a file or file system

# which
which <executable> - shows path to executable being used for command

# tput
tput - initialize a terminal or query terminfo database

# test
test - test <option> <args> - -e (exists) - see man for many options
also [ test params ]; - needs followed by ;

# expr
expr - evaluate an expression using normal operators

# hostname
hostname - evaluates the system hostname (i.e. users-computer.local), similar result to echo $HOSTNAME

# launchctl
launchctl - mac specific daemon runner for managing processes on startup

# pax
pax - read and write file archives and copy file hierarchies

# sleep
sleep - suspend execution for a period of time

# stty
stty - set options for terminal device interface

# sync
sync - flushes pending disk writes before the processor is halted (similar to shutdown)

# unlink
unlink - remove a symlink

# wait4path
wait4path - wait for given path to show up inside a namespace (i.e. directory or disk). If path exists,
script exits. Else script sleeps indefinitely until the path exists.

# sort
sort - sort lines of all concatenated files and print to standard out

# uniq
uniq - report or filter out repeated lines in a file

# whois
whois - Internet domain name and network number directory service lookup

# whereis
whereis - reports location of program/executable

# whatis
whatis - search system commands

# whoami
whoami - display current user id

# trace
trace - configure and record kernel trace events

# Additional Tools
diff
patch
crontab
readlink
tr
cut
xargs
expand
fmt
column
banner
bc - basic calculator - performs floating point math and other calculations not practical in shell math
expect - when working with scripts that require two-way interaction, expect provides facilities for more
complex interactions
expr - evaluates numerical expressions. Used for basic math and loop interation
awk - used for text processing with regular expressions
head - prints the first few lines from a file
sort - sort lines
tail - print the last few lines from a file
tee - copies stdin to stdout
tr - replaces on char with another
uniq - filters out adjacent lines that match
mkfifo - creates named pipes for communication, useful for connecting two tools in a circular fashion
diskutil, fsck, fsck_msdos, fsck_hfs, mount, unmount, mount_afp, mount_cd9660, mount_cddafs, mount_fdesc,
mount_ftp, mount_hfs, mount_msdos, mount_nfs, mount_ntfs, mount_smbfs, mount_udf, mount_url, mount_webdav
bzip, compress, gzip, zip, tar

# sh vs. bash
sh - specifies a programming language that meets POSIX standards. Bash and other shells are implementations
of this standard. On most POSIX systems, sh is a hard link to the shell.
bash - extensions to bash have moved it outside of strict POSIX compatibility. Supports a --posix switch
 #! - specifies which executable to use - if the system definitely has bash use bash, else use sh

# Redirection
cmd_output > - redirect stdout of cmd_output to a file
> - (when passed a file name and no preceding cmds) truncates file
cmd_output >> - appends to file
&>  >& - both redirect stdout and stderr to the same stream, semantically &> is preferred
cmd < FILENAME - read input from a file
<> - open a file for reading and writing
|& - redirect standard error to pipe

# Flow control
if -
if [ test ] ; then
   execute
elif [ test ] ; then
   execute
else
   execute
fi

test or [ ] both are commands (man \\[ ]) they are linked.
   the space between equals signs is critical [ arg1 = arg2 ] (i.e. difference  between assignment and comparison)
   spaces around brackets are also critical - the open bracket is a command that expects the final arg to be
   the close bracket
   quoting the variables makes sure to avoid errors if an empty variable is passed and also avoids errors
   if the variable contains spaces

while -
while true ; do
   execute
done

for i in list ; do
   execute
done

case expression in
   ( val ) execute ;;
   ( val ) execute ;;
   ( * ) execute ;;
esac

# expr
expr - performs various string comparisons and integer math

# Special Characters
<RETURN> - execute command
\# - comment
<SPACE> - argument separator
` - command substitution`
" - weak quotes
' - strong quotes
\ - single quote character
<variable>
| - pipe
^ - pipe
& - run program in background
? - match one character or test operator
* - match any number of characters, or wild card filename expansion through globbing
; - command separator, run commands sequentially
;; - end of case statement
~ - home dir
~user - user's home dir
! - history of commands, or not in some commands
- - start of optional argument
$# - number of arguments to script
$* - arguments to script*
$@ - original arguments to script
$- - flags passed to shell
$? - status of previous command
$$ - process identification number
$! - PID of last background job
&& - short circuit and, execute next command if and only if previous returns
|| - short circuit or, execute command following if and only if previous exits with a non-zero exit status
[] - match range of characters or test
%<job> - identifies job number
(cmd;cmd) - runs commands as a sub-shell
{} - in-line expansions, i.e. string-interpolation when used as ${variable}, also filename expansion (i.e. command operates on all files listed within, supports ranges {a..x})
{cmd;cmd} - like (cmd;cmd) wihtout a sub-shell
, - link series of arithmetic operations, return the last one, or string substitution
: - null command

# Special Characters Explained
filename expansion is globbing i.e. speical characters that are used to represent filename variables or groups of filename variables
$ - The primary means of shell script variable expansion. Variable names that are preceded by $ are expanded, even within " double quotes.
If used outside of double quotes, any additional globbing characters are expanded. (Like a function that operates on the variables to expose the
contents)

* - wildcard character. Matches any number of any character used within a filename
? - single wildcard character. Matches any single character within a filename.
{} - matches any series of a selection of options within a filename i.e. file.{jpg,gif} etc.
[] - matches any of a series of characters within a filename - like regex
() - Outlines a subroutine
   Group a chain of operations
   Used for math in some bourne variants
   Used in for loop iterators in some bourne variants
"" - Disables argument splitting on word boundaries (spaces) and shell expansion of most sepcial characters
   Variables are expanded wthin double quotes, but additional globbing wthin variables is not expanded
   Inline execution is also expanded
   \ still functions within bourne shell variants but not C shell
'' - disables argument splitting on word boundaries and all shell expansion. \ is treated as a literal
` - `this is roughly equivalent to $(), i.e. expand the results of a subroutine
\ - overrides special characters to be treated as literals, either within "" or just a script

# Quotation mark types
` -` used for command substitution
\ - single character quote or escape, i.e. \n, \(space) interprets the character literally
' - used for strong quoting, i.e. all of the characters are interpreted literally (string literal)
" - weak quotes, doesn't expand meta characters like * or ? but does expand variables and sub-commands
testing strings/quotes - use echo
Note - single quotes are safer than double quotes because shell does not do any interpretation on
the contents of the string

# Subroutine Basics
subroutine_name()
{
   execute
}
Looks like C functions but without argument lists. Arguments passed to the subroutine when called are always vriable arity
and can be referenced within the subroutine definition as $1, $2 (just like the outer shell script)
exit - exits entire script
# Anonymous Subroutines
{ routine 1 ; routine 2 ; } - the semi-colons are necesary


# HERE IS documents
create multi-line inputs for commands using <<(special word) text...\n...\n etc (special word)
supports variable expansion
to disable variable expansion and interpret the string literally use <<\(special word)

# Bourne Shell Variable Expansion
sh -v script
sh -x script
inside bash -
set -v (set verbose flag)
set -x (set echo variable)
tunr off -
set +v
set +x


# Keybd Shortcuts
Ctrl + A Go to the beginning of the line you are currently typing on
Ctrl + E Go to the end of the line you are currently typing on
Ctrl + L Clears the Screen, similar to the clear command
Ctrl + U Clears the line before the cursor position. If you are at the end of the line, clears the entire line.
Ctrl + H Same as backspace
Ctrl + R Let’s you search through previously used commands
Ctrl + C Kill whatever you are running
Ctrl + D Exit the current shell
Ctrl + Z Puts whatever you are running into a suspended background process. fg restores it.
Ctrl + W Delete the word before the cursor
Ctrl + K Clear the line after the cursor
Ctrl + T Swap the last two characters before the cursor
Esc + T	Swap the last two words before the cursor
Alt + F	Move cursor forward one word on the current line
Alt + B	Move cursor backward one word on the current line
Tab Auto-complete files and folder names

Alt is esc on mac

# Unix Networking
- OSI Model (Open Systems Interconnection Model)
   - application
   - presentation
   - session
   - transport
   - network
   - data link
   - physical
- IP Naming
   - correlation can be assigned within a Unix system that maps host name to IP
   - stored in /etc/hosts on a local host

# Unix Client/Server Model
- uses sockets
- one process, server, creates a socket which is known by other client process
- client creates an unnamed socket and then requests that it be connected to the server's named socket
- successful connection returns one file descriptor (file being POSIX related int that serves as an index
into a table maintained by the kernel that tracks open processes for IO) to both the client and one to server
used for reading and writing

# Commands
rcp - remote copy (between unix hosts)
scp - secure remote copy
rlogin - remote unix host login
rsh - remote shell
secure shell - remote login shell
ftp - file transfer protocol (does not have to be unix only)
telnet - allows connections between remote hosts with telnet servers
rwho -
finger - get info on a user on any machine within your network
ruptime - get status of machine on network
ping - check if a remote machine is up by sending an ICMP echo message (one packet) to host, does not tell if other daemons are up
arp - internet to ethernet address tranlation table display and modification for address resolution protocol
netstat - displays running error stats + counts on config interface every N seconds
   -a - display socket ports and states
   -s - protocol counts and errors
   -r - routing table dump
   -i - list of interface names
   -n - use numeric host lookup to bypass DNS lookup
ifconfig
   -a - show all interfaces
   <if_name> - current setup for a particular interface
   <if_name> <params> - set params for interface (when running in root only). The IP, subnet, etc are set on boot in /etc/rc
route ... params - the route command is used for setting static route paths in the routing table
routed - bsd routing daemon
gated - alternative routing daemon
traceroute - IP packet route tracing
nslookup - Translates IP to DNS or vice versa
inetd - mother daemon the listens to running apps listed in /etc/inted.conf
sendmail - runs SMTP protocol

# Important Files
/etc/hosts - name to ip mapping for localhost
/etc/networks - network name to ip address mapping
/etc/protocols - protocol names to protocol number mapping
/etc/services - tcp/udp service name to port numbers


# Additional Notes
show hidden files mac -
defaults write com.apple.finder AppleShowAllFiles TRUE

killall Finder

# Command Substitution
` ` vs $( ) - both generate output within a subshell - stdout data is what the substitution expands to 
The second form `COMMAND` is more or less obsolete for Bash, since it has some trouble with nesting ("inner" backticks need to be escaped) and escaping characters. Use $(COMMAND), which is posix compliant
