#### Mode

`Esc`: normal mode

`v`: visual mode ==visual==

`Shift+v`: visual line mode

`Ctrl+v`: visual block mode



#### Exiting

`:w`: save ==write==

`:q`: quit ==quit==

`:wq`/`ZZ`: save and quit

`:q!`: quit without saving

`:wa`: save all ==write all==

`:wqa`: save and quit all ==write quit all==



#### Edit

`i`: insert before cursor ==insert==

`I`: insert at the start of line

`a`: append after cursor ==append==

`A`: append at the end of line

`o`: append a new line below ==open==

`O`: append a new line above

`s`: substitute character ==substitute==

`S`: substitute the whole line



`x`: delete the current character ==cut==

`dd`: delete the current line ==delete==

`d0`: delete before cursor

`D`/`d$`: delete after cursor

`d <n> b/w`: delete `n` words before / after cursor

`:<n>d` delete line `<n>`



`yy`: copy current line ==yank==

`<n>p`: paste`<n>` times ==paste==



`u`: undo ==undo==

`Ctrl+r`: redo ==redo==

`gg=G`: fix indentation

`:r! <command>`: run unix shell commands



#### Cursor Movement

`h j k l`: ←↓↑→

`b`/`w`: previous / next word

`(`/`)`: previous / next sentence

`{`/`}`: previous / next paragraph

`%`: matching parenthesis/bracket/curly brace

`<n><movement>`: apply `<movement>` (`b`, `w`, `k`, `j`) `<n>` times

`:<n>`: jump to line `<n>`

`gg`/`G`: first / last line



#### Find

`/<regex>`: find `<regex>`

`N`/`n`: previous / next occurrence

`:%s/<old word>/<new word>/gc`: substitute all words ==global confirm==



#### Layout

`vim -p <files>`: open files in different tabs

`gT`/`gt`: previous / next tab ==tab==

`:sp <file>`: split screen horizontally ==split==

`:vsp <file>`: split screen vertically ==vertical split==

`Ctrl+w+w`: go to another window

`Ctrl+w ... <direction>`: go to the window on `<direction>`

`Crtl+w+=`: equal window size

`Ctrl+H`/`Ctrl+J`: change to horizontal / vertical spilit

`Ctrl+R`: swop windows