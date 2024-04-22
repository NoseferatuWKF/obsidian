# Keybinds

general
```lua
-- modes
a -- append after cursor (insert)
i -- insert on cursor (insert)
I -- insert in the beginning of the line (insert)
A -- append in the end of the line (insert)
s -- remove on cursor and edit (insert)
S -- edit whole line (insert)
C -- edit line after cursor (insert)
o -- insert line below (insert)
O -- insert line above (insert)
r -- replace one on cursor (replace)
R -- replace until quit on cursor (replace)
v -- visual (visual)
V -- v-line (visual)
<C-v> -- v-block (visual)

-- numbers
<C-a> -- increment number
<C-x> -- decrement number
<C-r> -- once in insert mode to do math

-- 
<C-G>  -- print current buffer details
<C-z> -- suspend vim, resume with fg
```

netrw
```lua
t -- to add cursor to tab
% -- add new file
D -- delete file/dir
d -- add new dir
R -- rename/move file/dir
I -- show/hide banner
p -- preview file
mt -- mark target dir for copying
mf -- mark file for copying
mc -- copy file to target
```

navigations
```lua
gg -- go top of file
gj -- go down one line, good for wrapped lines, works with k,$,0 as well 
gv -- go to previous highlighted
gf -- go to file on cursor
gx -- go to url on cursor
g; -- changelist forward
g, -- changelist backward
G -- go bottom of file
H -- go top of window (limited by vim scrolloff)
M -- go middle of window (limited by vim scrolloff)
L -- go bottom of window (limited by vim scrolloff)
f -- forward first occurence (use ; or , to go back and forth)
F -- prev first occurence (use ; or , to go back and forth)
{ -- prev blank line
} -- next blank line
% -- go to correspoding bracket
<C-d> -- half page down
<C-u> -- half page up
<C-f> -- full page up
<C-b> -- full page up
<C-o> -- jumplist backward
<C-i> -- jumplist forward
/keyword -- search for keyword
* -- next occurence of word under cursor
# -- prev occurence of word under cursor
]m -- next method/function
[m -- prev method/function
```

motions
>applies to other operations not just yanking
```lua
yap -- yank paragraph
yy -- yank line
Y -- yank after cursor
yiw -- yank word on cursor
yi( -- yank inside () work with other wrappers except {}
ya( -- yank around(includes wrapper) () work with other wrappers except {}
~ -- reverse case on cursor
guu -- lowercase all in line
gUU -- uppercase all in line
g~~ -- reverse case all in line
gUfw -- uppercase until first 'w'
gqq -- make wrapped lines into proper lines
g& -- apply prev substitutions to whole file
g<C-a> -- increment all selection 
ctw -- change line before 'w'
cW -- change contiguous word
```

tabs
>doesn't work with buftabline
```lua
-- tabs
<C-w> T -- open new tab with current buffer
gt -- go to next tab
```

window
```lua
<C-w> v -- vertical split
<C-w> s -- horizontal split
<C-w> w -- alternate windows
<C-w> x -- swap window with next
<C-w> q -- close a window
<C-w> o -- close all windows except current
<C-w> = -- make all windows equal size
```

buffers
>use arglist if buffer gets to big
```lua
<C-^> -- switch between buffers
```

marks
>local marks are lowercase and global marks are caps
```lua
ma -- create a new local mark 'a'
mA -- create a new global mark 'A'
`a -- jump to 'a' mark (can use ' as well)
:`apu -- paste to 'a' mark
:`ad -- delete 'a' mark
: `apu | `ad -- replace marked line with paste buffer
```

registers and macros
```lua
qa -- record macro into named register 'a'
@a -- replay macro in named register 'a'
10@a -- replay macro in named register 'a' 10 times
+ -- system clipboard
* -- selection clipboard
```

# Commands

general
```lua
:e file                 -- edit file
:e!                     -- cancel editing
:!echo @%               -- directory/name of file
:!echo '%:t'            -- name of file ('tail')
:!echo '%:p'            -- full path
:!echo '%:p:h'          -- directory containing file ('head')
:cd %:p:h               -- change working directory for current window
:lcd %:p:h              -- change working directory for all windows
:%s/abc/def/g           -- replace all strings within file
:%s/\s\+$//g            -- remote all trailing white spaces
:g/^\s*$/d              -- delete white spaces can use :'<,'>s/^\s*// as well
:g/something/d          -- delete all lines containing something
:0,10 s/abc/def/g       -- replace line 0 - 10 strings
:-5,-10co.              -- copy relative line from -5 to -10
:-5,-10mo.              -- move relative line from -5 to -10
:30 put a               -- paste at line 30 from reg a
:checkhealth            -- check neovim config
:'<,'>!sort | uniq      -- sort and dedup selected lines
:'<,'>s/$/s             -- add s to the end of each line of selection
:%s/\(.*\)/\1\rabc/     -- add capture group and then add new line with abc
:s/\<four\>/4/g         -- change exact match of four to 4
:X                      -- encrypt/decrypt file (neovim cannot do this)
:args                   -- prints arglist
:arga file              -- add file to arglist
:term                   -- open terminal
:.!ls                   -- print files in current directory to current buffer
:%!python -m json.tool  -- format json
:!jq .                  -- format json
q:                      -- secret command
```

quickfixlist
```lua
:copen                  -- open quickfix list
:cnext                  -- next file in quickfix list
:cprev                  -- prev file in quickfix list
:call setqfixlist([])   -- clear quickfix list
:cdo s/a/b/             -- substitute a to b in quickfix list
```

# Clever Tricks

substitute in multiple files
```lua
vim *.cpp                     -- all all cpp files in current dir to arglist
qq                            -- start a macro recording in q register
%s/\<GetResp\>/GetAnswer/ge   -- change all occurence of GetResp (exact) to Get Answer, the e flag avoids the substitution throwing an error on not finding a match
:wnext                        -- go to next file
q                             -- stop recording maro
@q                            -- execute the q register, verify that its working
999@q                         -- repeat the q register 999 times
-- wnext can't move to the next file after the last file, so this stops the execution
```

find where word is used
```lua
-- grep -l only lists the file with matches frame_counter
-- *.c filters only c files
-- try it out with ripgrep as well
vim `grep -l frame_counter *.c`
```

# Plugins

telescope
```lua
vim_options -- show all vim options
commands -- show all neovim commands
help_tags -- show all declared global functionalities and vim apis
treesitter -- fast method, symbol lookup
git_branches -- fast branch checkouts
git_bcommits -- look at commit changes in current buffer
```

vim-fugitive
```lua
s -- stage
u -- unstage
U -- unstage all
= -- toggle diff
X -- restore file
cc -- create a commit
czz -- push stash
R -- reload status
q -- close status

-- rebase
ri -- rebase interactive
rr -- continue the current rebase
rs -- skip current rebase
ra -- abort the current rebase

-- resolving merge conflicts
dv -- split screen merge conflict
:diffget {buffer} -- could be //2 or //3 depending on which way to go
Gwrite! -- to use native vim fugitive merge resolution
```

vim-surround
```lua
cs      -- change surround
ysiw    -- add surround in word
ysap    -- add surround around paragraph
ds      -- delete surround
```

Mason
```lua
-- list all available lsp
:Mason
```

Treesitter
```lua
-- list all available parsers
:TSInstallInfo
```

vim to neovim options
```lua
-- in vim you might have something like this
-- let g:some_option = 1
vim.g.some_option = 1 -- neovim equivalent
```

# VIM API

```lua
-- Print lua table in neovim for debugging
print(vim.inspect(vim.api.nvim_get_win_config(0))) -- example for window config

-- Check if a file is readable
if vim.fn.filereadable == 1 then
	print("OK")
end

-- Check if buffer 1 exists
if vim.fn.buflisted(1) then
	print("OK")
end

-- get [count]
let count = vim.v.count

-- encode/decode json
local lua_table = vim.fn.json_decode("/path/to/file.json")
local json = vim.fn.json_encode(lua_table)

-- read vs readfile
vim.cmd.read("/path/to/file") -- read into buffer
vim.fn.readfile("/path/to/file") -- read into memory

-- event handler
vim.api.nvim_create_autocmd({"BufEnter"}, {
	pattern = "*.cs",
	callback = function(ev)
		print(ev)
	end
})
```