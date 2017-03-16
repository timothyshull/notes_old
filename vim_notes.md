A vim buffer is a file that is open for editing. Additionally, there are cases where buffers are
not associated with a file.
:edit <filename>
:new
:vnew
All create buffers
:buffers, :ls, :files - list open buffers
:bdelete - delete buffers
:[Num]b - go to buffer

# Cursor movement
- h - move cursor left
- j - move cursor down
- k - move cursor up
- l - move cursor right
- w - jump forwards to the start of a word
- W - jump forwards to the start of a word (words can contain punctuation)
- e - jump forwards to the end of a word
- E - jump forwards to the end of a word (words can contain punctuation)
- b - jump backwards to the start of a word
- B - jump backwards to the start of a word (words can contain punctuation)
- 0 - jump to the start of the line
- ^ - jump to the first non-blank character of the line
- $ - jump to the end of the line
- G - go to the last line of the document
- 5G - go to line 5
- Tip Prefix a cursor movement command with a number to repeat it. For example, 4j moves down 4 lines.

- 0   move to beginning of line
- $   move to end of line
- ^   move to first non-blank char of the line
- _   same as above, but can take a count to go to a different line
- g_i  move to last non-blank char of the line (can also take a count as above)

- gg  move to first line
- G   move to last line
- nG  move to n'th line of file (where n is a number)

- H   move to top of screen
- M   move to middle of screen
- L   move to bottom of screen

- z.  put the line with the cursor at the center
- zt  put the line with the cursor at the top
- zb  put the line with the cursor at the bottom of the screen

- Ctrl-D  move half-page down
- Ctrl-U  move half-page up
- Ctrl-B  page up
- Ctrl-F  page down
- Ctrl-o  jump to last cursor position
- Ctrl-i  jump to next cursor position

- n   next matching search pattern
- N   previous matching search pattern
- *   next word under cursor
- #   previous word under cursor
- g *  next matching search pattern under cursor
- g#  previous matching search pattern under cursor

- %   jump to matching bracket { } [ ] ( )

# Insert mode - inserting/appending text
- i - insert before the cursor
- I - insert at the beginning of the line
- a - insert (append) after the cursor
- A - insert (append) at the end of the line
- o - append (open) a new line below the current line
- O - append (open) a new line above the current line
- ea - insert (append) at the end of the word
- Esc - exit insert mode
- <C h> - delete one character in insert mode
- <C w> - delete back one word
- <C u> - delete to start of line

# Insert normal mode
Within insert mode - <C-o>
This allows you to enter one single command and then instantly returns to insert mode

# Editing
- r - replace a single character
- J - join line below to the current one
- cc - change (replace) entire line
- cw - change (replace) to the end of the word
- c$ - change (replace) to the end of the line
- s - delete character and substitute text
- S - delete line and substitute text (same as cc)
- xp - transpose two letters (delete and paste)
- u - undo
- Ctrl + r - redo
- . - repeat last command
- Marking text (visual mode)
- v - start visual mode, mark lines, then do a command (like y-yank)
- V - start linewise visual mode
- o - move to other end of marked area
- Ctrl + v - start visual block mode
- O - move to other corner of block
- aw - mark a word
- ab - a block with ()
- aB - a block with {}
- ib - inner block with ()
- iB - inner block with {}
- Esc - exit visual mode

# Visual commands
- > - shift text right
- < - shift text left
- y - yank (copy) marked text
- d - delete marked text
- ~ - switch case
- Cut and paste
- yy - yank (copy) a line
- 2yy - yank (copy) 2 lines
- yw - yank (copy) word
- y$ - yank (copy) to end of line
- p - put (paste) the clipboard after cursor
- P - put (paste) before cursor
- dd - delete (cut) a line
- 2dd - delete (cut) 2 lines
- dw - delete (cut) word
- D - delete (cut) to the end of the line
- d$ - delete (cut) to the end of the line
- x - delete (cut) character
Notes - three variants - working with characters, lines, or blocks of text
From normal mode or within visual mode to switch -
v - enter character-wise visual mode - this mode is for highlighting and operating on single characters
V - enter line wise visual mode - highlight and operate on full lines at a time
<C-v> - enter block-wise visual mode - like option and select in text mate, operates in blocks by row and column
Operate on line endings - enter visual block mode and press $, after selection is made select A, type text, then return to normal mode

# Select mode
Enter select mode from visual mode with <C-g>
Select mode functions more like other text editors, i.e. it replaces the selected text upon typing

# Command line mode
- Allows entry of ex command, a search pattern, or an expression
- Esc enters command, <Cr> executes commands,
- up and down cycles through previous commands
- enter the command window with q: - pressing <C-r> executes the command from the current line at the end
- within command line mode, we can execute external commands by prefixing them with !
- temporarily leave vim and enter the shell with :shell and exit to return to vim
- pressing Ctl-z within a terminal vim session puts the vim process in the background, use fg to resume suspended job
- :read !{cmd} - execute the command and put the contents below the cursor
- :[range]wrtie !{cmd} - execute cmd in the shell with the selected range as the input
- :[range]!{filter} - filter the specified range through an external command

# Ex commands
:[range]delete [x] - delete specified lines (into register x)
:[range]yank [x] - yank lines (into register)
:[line]put [x] - put line into register x
copy
move
join
normal
substitute
global
:t - duplicate lines
:m - move lines

# Ranges
num - line numbered num
1 - first line
$ - last line
. - current line
% - all lines
num1, num2 - lines num1 to num2 inclusive
num1, $ - lines num1 to end
., $ - current line to end
.+num,.+num - delete lines from current line
.num,.num - same as above

# Exiting
- :w - write (save) the file, but don't exit
- :wq or :x or ZZ - write (save) and quit
- :q - quit (fails if there are unsaved changes)
- :q! or ZQ - quit and throw away unsaved changes
- Search and replace
- /pattern - search for pattern
- ?pattern - search backward for pattern
- n - repeat search in same direction
- N - repeat search in opposite direction
- :%s/old/new/g - replace all old with new throughout file
- :%s/old/new/gc - replace all old with new throughout file with confirmations
- Working with multiple files
- :e filename - edit a file in a new buffer
- :bnext or :bn - go to the next buffer
- :bprev or :bp - go to the previous buffer
- :bd - delete a buffer (close a file)
- :sp filename - open a file in a new buffer and split window
- :vsp filename - open a file in a new buffer and vertically split window
- Ctrl + ws - split window
- Ctrl + ww - switch windows
- Ctrl + wq - quit a window
- Ctrl + wv - split window vertically
- Ctrl + wh - move cursor to the left window (vertical split)
- Ctrl + wl - move cursor to the right window (vertical split)
- Ctrl + wj - move cursor to the window below (horizontal split)
- Ctrl + wk - move cursor to the window above (horizontal split)

# Tabs
- :tabnew filename or :tabn filename - open a file in a new tab
- Ctrl + wT - move the current split window into its own tab
- gt or :tabnext or :tabn - move to the next tab
- gT or :tabprev or :tabp - move to the previous tab
- #gt - move to tab number #
- :tabmove # - move current tab to the #th position (indexed from 0)
- :tabclose or :tabc - close the current tab and all its windows
- :tabonly or :tabo - close all tabs except for the current one

up - k
down - j
left - h
right - l
esc - return to normal mode
x - delete single
dw - delete word (to end of next word)
de - to the end of the current word, INCLUDING the last character.
d$ - delete line (from beginning of line)
i - insert
A - append (only difference is insert moves before and append moves after current position)

paste from system clipboard - “ * or “+ plus p

q! - exit
wq - write


writing in multiple places - enter visual block mode with ctrl + v, move to locations, select I, type text, press esc

Ex commands :
search patterns / ?
filter command !

. - repeat lest recorded set of keystrokes (in context of mode i.e. normal or insert)
a/A - append after current cursor position/end of current line
set number or :set number - show line numbers
yyp - copy a line and repeat on next line
daw - delete word to beginning when at end
d{motion},c{motion},y{motion} - use these to compose actions
c is change, d is delete, y is copy
g~ - toggle case with movement
gU - uppercase with movement
gu - lowercase with movement
> - shift right
< - shift left
= - autoindent
! - Filter {motion} lines through an external program

R - enter replace mode, allows you to avoid deleting a rewriting
<C-r>{register} - paste from register
<C-r><C-p>{register} - fixes unintended indentation
<C-[> - same as esc
:registers - view contents of registers
"% - name of current file
"# - name of alternate file
". - last inserted text
": - last ex command
"/ - last search pattern

# Insert special chars
<C-v>{123} - Insert by decimal code
<C-v>u{1234} - insert by hex code
<C-v>{nondigit} - Insert literal nondigit
<C-k>{char1}{char2} - Insert character represented by char1 char2 digraph

/search - search for a string, cycle through (repeat) with n, cycle backwards with N
pres : or / plus up or down - cycle through command line history
