# VIM Commands

## Usefull keys and sequences

### Insert Text
```
a          append right to the cursor  
A          append right to the line  
ea         append at end of word  
i          insert left to the cursor  
I          insert at beginning of line  
s          substitute character  
S          substitute line  
o          insert line below cursor  
O          insert line above cursor  
```

### Moving the cursor
```
H          to beginning of page  
M          to middle of page  
L          to end of page  
gg         to the beginning of the document  
G          to the end of the document  
$          to the end of the line  
0          to the geginning of line  
^          to the beginning of line (first non space char)  
h          left (one character)  
l          right (one character)  
j          one line down  
k          one line up  
4k         four lines up  
13G        instantly to line 13  
w          to beginning of next word  
e          to end of next word  
b          to beginning of previous word  
3w         to the beginning of the next third word  
<C - e>    move screen one line up  
<C - y>    move screen one line down  
```

### Deleting
```
x          character right to the cursor
X          character left to the cursor
diw        the current word where the cursor is over
dd         the current line (where the cursor is)
ggdG       the whole document (you can also use :%d)
3dd        the next three lines (incl. the cursors line)
dgg        all lines from cursor to the documents beginning
dG         all Lines from the cursor to the documents end
dw         from cursor to end of word
d3w        from cursor the next three words
di"        inside "", you can also use di' di( di{ di[ to delete inside
dit        all content inside two tags like <b> blabla</b>
dtX        from cursor to next X (excluding X)
dfX        from cursor to next X (including X)
dTX        from cursor to previous X (excluding X)
dFX        from cursor to previous X (including X)
cit        delete inside tags and write immediately something
```

### Marking, Copy and paste
```
*          highlight all words, like that under the cursor
V          visually mark the whole current line
viw        visually mark word under cursor
p          paste what has been copied before
4p         paste what has been copied before 4 times
yy         copy whole line where the cursor is
Y          copy whole line where the cursor is
3Y         copy three lines, from the line where the cursor is
yiw        copy word under cursor
yi"        copy text between ". Works for (, [, {, ', < too
ya"        copy text between quotes Works for (, [, {, ', < too
```

### Search and replace
```
replace all foo with bar (% = in all lines, s=substitute)
:%s/foo/bar/g

replace all foo with bar (c = ask for confirmation before)
:%s/foo/bar/gc

list the number of the occurence of searchterm
:%s/searchstring//gn

replace lines start with 123 with aaa, from line 10 to 25aaa
:10,25 s/^123/aaa/g

my/old with my/new. To avoid masking specialcharacters use: s:my/old:my/new/g
:% s/my\/old/my\/new/g

remove all leading spaces in all lines
:%le

delete everithing before the pattern http
:s/^.*\(FOO\)/\1/
```

### Working with files
```
:ls                      list all buffers (files) currently open
:bnext                   goto next buffer
:bprevious               goto previous buffer
:buffer X                jump to buffer X (get buffer-number with :ls command)
```

### Working with windows
```
<C – w>s                 splits the current window horizontally
<C – w>v                 splits the current window verically
<C – w>c                 close the current window
<C – w>w                 walk through the windows
<C – w>=                 fit all windows to the same size
<C -w>T                  moves the current window into a new Tab
```

### Working with Tabs
```
:tabnew                  create a new tab
:tabclose                closes the current tab
gt                       goto next tab
:tabnext                 goto next tab
gT                       goto previous tab
:tabprevious             goto previous tab
:map <F5> gt             map „goto next tab“ to the F5 key
```

### Sorting and manipulating the contents structure
```
:sort                    sort all lines alphabetically
:sort u                  sort all lines alphabetically and remove duplicates
ggVGu                    lowercase whole Text
```

### Shortkeys
```
fe                       go to next letter "e" in the current line
Fe                       go to pevious letter "e" in current line
;                        go to next occurence of what you found with t/T/f/F :h f        show the vim internal help for f
<C-a>                    increase a marked number
<C-x>                    decrease a marked number
J                        hang next line to the end of current line
```

## Settings
```
show all visible characters like spaces, tabs, line breaks / hide all visible characters
set list
set nolist

Enable / Disable syntax highlighting
syntax on
syntax off

Enabling current row and column highlighting
set cursorline
set cursorcolumn

Enabling relative line numbering (cursor is always at zero)
set relativenumber

Indentation in vi
you want to mark something in visual mode, and indent the selected part several times to the left or the right?
First, you add these lines to your .vimrc
vnoremap < <gv
vnoremap > >gv
Now you can visually select a text passage (press v move the arrow keys), afterward, indent it to the left by pressing < or indent it to the right by pressing >

```

## Plugins
### CtrlP
```
CtrlP allows you, to easily select files and open them in Tabs or windows

<C - f>         Open file search
<C - z>         Mark multiple files (in CtrlP)
<C - o>         Open marked files (in CtrlP)
<C - t>         Open marked files in new tabs (in CtrlP)
<C - v>         Open marked files verticaly split(in CtrlP)
<C - x>         Open marked files horizontaly slpit (in CtrlP)
```
