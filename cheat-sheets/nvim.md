## vanilla keybinds

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

-- buffers
{count}CTRL-G  -- print buffer absolute file path
```

netrw
```lua
t -- to add cursor to tab
% -- add new file
D -- delete file/dir
d -- add new dir
R -- rename/move file/dir
I -- show/hide banner
```

navigations
```lua
gg -- go top of file
gj -- go down one line, good for wrapped lines, works with k,$,0 as well 
gv -- go to previous highlighted
gf -- go to file on cursor
gx -- go to url on cursor
G -- go bottom of file
H -- go top of window (limited by vim scrolloff)
M -- go middle of window (limited by vim scrolloff)
L -- go bottom of window (limited by vim scrolloff)
f -- forward first occurence
F -- prev first occurence
{ -- prev blank line
} -- next blank line
<C-d> -- half page down
<C-u> -- half page up
/keyword -- search for keyword
* -- next occurence of word under cursor
# -- prev occurence of word under cursor
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
ctw -- change line before 'w'
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
<C-w> x -- swap window with next
<C-w> q -- close a window
<C-w> o -- close all windows except current
<C-w> = -- make all windows equal size
```

marks
>local marks are lowercase and global marks are caps
```lua
ma -- create a new local mark 'a'
mA -- create a new global mark 'A'
`a -- jump to 'a' mark
```

registers and macros
```lua
qa -- record macro into register 'a'
@a -- replay macro in register 'a'
10@a -- replay macro in register 'a' 10 times
+ -- system clipboard
* -- selection clipboard
```

## commands
```lua
:e file                 -- edit file
:!echo @%               -- directory/name of file
:!echo expand('%:t')    -- name of file ('tail')
:!echo expand('%:p')    -- full path
:!echo expand('%:p:h')  -- directory containing file ('head')
:cd %:p:h               -- change working directory for current window
:lcd %:p:h              -- change working directory for all windows
:%s/abc/def/g           -- replace all strings within file
:0,10 s/abc/def/g       -- replace line 0 - 10 strings
:-5,-10co.              -- copy relative line from -5 to -10
:checkhealth            -- check nvim config
:'<,'>!sort | uniq      -- sort and dedup selected lines
:X                      -- encrypt/decrypt file
```

## mah keybinds

```lua
-- Netrw
vim.keymap.set("n", "<leader>pv", vim.cmd.Ex)
```

## some of ThePrimeagen keybinds

```lua
-- Notorious keybinds from ThePrimeagen
-- moving lines
vim.keymap.set("v", "J", ":m '>+1<CR>gv=gv")
vim.keymap.set("v", "K", ":m '<-2<CR>gv=gv")
-- removing disorientation
vim.keymap.set("n", "J", "mzJ`z")
vim.keymap.set("n", "<C-d>", "<C-d>zz")
vim.keymap.set("n", "<C-u>", "<C-u>zz")
vim.keymap.set("n", "n", "nzzzv")
vim.keymap.set("n", "N", "Nzzzv")
-- save current copy buffer
vim.keymap.set("x", "<leader>p", [["_dP]])
vim.keymap.set("n", "Q", "<nop>")
-- replace word on cursor
vim.keymap.set("n", "<leader>s", [[:%s/\<<C-r><C-w>\>/<C-r><C-w>/gI<Left><Left><Left>]])

```

## nvim-lspconfig

```lua
vim.keymap.set('n', '<space>e', vim.diagnostic.open_float, opts)
vim.keymap.set('n', '[d', vim.diagnostic.goto_prev, opts)
vim.keymap.set('n', ']d', vim.diagnostic.goto_next, opts)
vim.keymap.set('n', '<space>q', vim.diagnostic.setloclist, opts)
```

## lsp

```lua
  local nmap = function(keys, func, desc)
    if desc then
      desc = "LSP: " .. desc
    end

    vim.keymap.set("n", keys, func, { buffer = bufnr, desc = desc })
  end

  nmap("<leader>rn", vim.lsp.buf.rename, "[R]e[n]ame")
  nmap("<leader>ca", vim.lsp.buf.code_action, "[C]ode [A]ction")

  nmap("gd", vim.lsp.buf.definition, "[G]oto [D]efinition")
  nmap("gr", require("telescope.builtin").lsp_references, "[G]oto [R]eferences")
  nmap("gI", vim.lsp.buf.implementation, "[G]oto [I]mplementation")
  nmap("<leader>D", vim.lsp.buf.type_definition, "Type [D]efinition")
  nmap("<leader>ds", require("telescope.builtin").lsp_document_symbols, "[D]ocument [S]ymbols")
  nmap("<leader>ws", require("telescope.builtin").lsp_dynamic_workspace_symbols, "[W]orkspace [S]ymbols")

  -- See `:help K` for why this keymap
  nmap("K", vim.lsp.buf.hover, "Hover Documentation")
  nmap("<C-k>", vim.lsp.buf.signature_help, "Signature Documentation")

  -- Lesser used LSP functionality
  nmap("gD", vim.lsp.buf.declaration, "[G]oto [D]eclaration")
  nmap("<leader>wa", vim.lsp.buf.add_workspace_folder, "[W]orkspace [A]dd Folder")
  nmap("<leader>wr", vim.lsp.buf.remove_workspace_folder, "[W]orkspace [R]emove Folder")
  nmap("<leader>wl", function()
    print(vim.inspect(vim.lsp.buf.list_workspace_folders()))
  end, "[W]orkspace [L]ist Folders")

  -- Create a command `:Format` local to the LSP buffer
  vim.api.nvim_buf_create_user_command(bufnr, "Format", function(_)
    vim.lsp.buf.format()
  end, { desc = "Format current buffer with LSP" })

```

## [trouble](https://github.com/folke/trouble.nvim)
>not sure should install or not, currently on lua diagnostics and working well so far

```lua
vim.keymap.set("n", "<leader>xx", "<cmd>TroubleToggle<cr>",
  {silent = true, noremap = true}
)
vim.keymap.set("n", "<leader>xw", "<cmd>TroubleToggle workspace_diagnostics<cr>",
  {silent = true, noremap = true}
)
vim.keymap.set("n", "<leader>xd", "<cmd>TroubleToggle document_diagnostics<cr>",
  {silent = true, noremap = true}
)
vim.keymap.set("n", "<leader>xl", "<cmd>TroubleToggle loclist<cr>",
  {silent = true, noremap = true}
)
vim.keymap.set("n", "<leader>xq", "<cmd>TroubleToggle quickfix<cr>",
  {silent = true, noremap = true}
)
vim.keymap.set("n", "gR", "<cmd>TroubleToggle lsp_references<cr>",
  {silent = true, noremap = true}
)
```

## [nvim-dap](https://github.com/mfussenegger/nvim-dap/tree/master)
```lua
vim.keymap.set('n', '<F5>', function() require('dap').continue() end)
vim.keymap.set('n', '<F10>', function() require('dap').step_over() end)
vim.keymap.set('n', '<F11>', function() require('dap').step_into() end)
vim.keymap.set('n', '<F12>', function() require('dap').step_out() end)
vim.keymap.set('n', '<Leader>b', function() require('dap').toggle_breakpoint() end)
vim.keymap.set('n', '<Leader>B', function() require('dap').set_breakpoint() end)
vim.keymap.set('n', '<Leader>lp', function() require('dap').set_breakpoint(nil, nil, vim.fn.input('Log point message: ')) end)
vim.keymap.set('n', '<Leader>dr', function() require('dap').repl.open() end)
vim.keymap.set('n', '<Leader>dl', function() require('dap').run_last() end)
vim.keymap.set({'n', 'v'}, '<Leader>dh', function()
  require('dap.ui.widgets').hover()
end)
vim.keymap.set({'n', 'v'}, '<Leader>dp', function()
  require('dap.ui.widgets').preview()
end)
vim.keymap.set('n', '<Leader>df', function()
  local widgets = require('dap.ui.widgets')
  widgets.centered_float(widgets.frames)
end)
vim.keymap.set('n', '<Leader>ds', function()
  local widgets = require('dap.ui.widgets')
  widgets.centered_float(widgets.scopes)
end)
```

## vim-surround

```lua
cs '" -- 'text' -> "text" 
ds " -- "text" -> text
(visual) S" -- text -> "text"
```

## [telescope](https://github.com/nvim-telescope/telescope.nvim)

## vim-fugitive

```lua
s -- stage
u -- unstage
U -- unstage all
= -- toggle diff

cc -- create a commit
```