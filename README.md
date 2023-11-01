# ⚠️  NOTE: THIS IS A FORK

This is a fork of the original symbols-outline.nvim which merges some selected
PRs from the original repo, plus some other improvements due to personal
preferences.

It does not attempt to be an up-to-date successor of the original repo, nor does
it attempt to ensure everyone's use-cases are satisfied by providing an overall
better experience. It is merely a fork which I maintain for my personal
use-cases which incorporates some selected PRs.

## Maintenance status

This fork is NOT guaranteed to be completely bug-free, nor as stable as the
original. However, since I use this plugin myself, it is guaranteed that
selected issues that I encounter myself would be fixed (to the best of my
ability).

I do not merge PRs from the original repo that I don't personally need.

- **DO use this fork if**:
  - You want to use features available in this fork, which are not included
  upstream (listed below)
  - You MIGHT want some up-to-date fixes to problems (that I also face) but is
  OK with some things not being looked after well (things I don't personally use)

- **Do NOT use this fork if**:
  - You want a stable plugin
  - You don't need the extra features in this fork

## Features

Below is a list of features I've included in this fork which, at the time of
writing, has not been included upstream (in the original repo). I try my best to
keep this list up to date.

- Feat: Toggling folds (and added default keymaps for it)
(simrat39/symbols-outline.nvim#194)
- Feat: when `auto_close=true` only auto close if `goto_location` is used (where
focus changed), and not for `focus_location` (simrat39/symbols-outline.nvim#119)

- Fix `SymbolsOutlineClose` crashing when already closed: simrat39/symbols-outline.nvim#163 (simrat39/symbols-outline.nvim#168)
- Support Nerd fonts v3.0: simrat39/symbols-outline.nvim#225
- Fix newlines in symbols crash: simrat39/symbols-outline.nvim#204 (simrat39/symbols-outline.nvim#184)
- Fix `code_actions`: simrat39/symbols-outline.nvim#168 (simrat39/symbols-outline.nvim#123)
- Fix fold all operation too slow: simrat39/symbols-outline.nvim#223 (simrat39/symbols-outline.nvim#224)

### PRs

- Open handler checks if view is not already open
  #235 opened on Sep 1 by eyalz800

- auto_jump config param
  #229 opened on Jun 8 by stickperson

- [x] Update nerd fonts to 3.0
  #225 opened on May 24 by anstadnik

- [x] fix(folding): optimize fold/unfold all
  #223 opened on May 22 by wjdwndud0114

- [x] Support markdown setext-style headers
  #222 opened on May 19 by msr1k

- [x] fix(icons): replace obsolete icons
  #219 opened on May 2 by loichyan
  **Superseded by #225**

- [x] Support ccls symbols
  #218 opened on Apr 25 by rqdmap

- [x] fix: replace newlines with spaces in writer
  #204 opened on Feb 15 by tbung

- [x] Make close_outline idempotent
  #200 opened on Feb 1 by showermat
  **Superseded by #163**

- Distinguish between public and private function display in Elixir
  #187 opened on Oct 25, 2022 by scottming

- [x] Fix some options
  #180 opened on Oct 7, 2022 by cljoly

- fix: Invalid buffer id error
  #177 opened on Sep 30, 2022 by snowair

- [x] fix(code_actions): use the builtin code_action
  #168 opened on Sep 14, 2022 by zjp-CN

- [x] fix: plugin crashes when SymbolOutlineClose used
  #163 opened on Aug 31, 2022 by beauwilliams

- feat: Add window_bg_highlight to config
  #137 opened on Jun 3, 2022 by Zane-

- Added preview width and relative size
  #130 opened on Apr 22, 2022 by Freyskeyd

- Improve preview, hover windows configurability and looks
  #128 opened on Apr 22, 2022 by toppair

- [x] Do not close outline when focus_location occurs
  #119 opened on Mar 18, 2022 by M1Sports20

- feat: instant_preview
  #116 opened on Mar 12, 2022 by axieax

- check if code_win is nill
  #110 opened on Feb 12, 2022 by i3Cheese

- Floating window (Draft)
  #101 opened on Jan 30, 2022 by druskus20


### TODO

KEY:
```
- [ ] : Planned
- [/] : WIP
-     : Idea
```

Items will be moved to above list when complete.

- Folds
  - [ ] Org-like <kbd>shift+tab</kbd> behavior: Open folds level-by-level
  - [ ] Optionally not opening all child nodes when opening parent node
  - Fold siblings and siblings of parent on startup
- Navigation
  - Go to parent
  - Cycle siblings

### Related plugins

- nvim-navic
- nvim-navbuddy
- dropdown.nvim
- treesitter (inspect/edit)

---

# symbols-outline.nvim

**A tree like view for symbols in Neovim using the Language Server Protocol.
Supports all your favourite languages.**

![demo](https://github.com/simrat39/rust-tools-demos/raw/master/symbols-demo.gif)

## Prerequisites

- `neovim 0.7+`
- Properly configured Neovim LSP client

## Installation

Use `hedyhli/symbols-outline.nvim` if you wish to use this fork.

Packer:
```lua
use 'simrat39/symbols-outline.nvim'
```

Lazy:
```lua
{
  "simrat39/symbols-outline.nvim",
  config = function()
    -- Example mapping to toggle symbols-outline
    vim.keymap.set("n", "<leader>tt", "<cmd>SymbolsOutline<CR>",
      { desc = "SymbolsOutline" })

    require("symbols-outline").setup {
      -- Your setup opts here (optional)
    }
  end,
},
```

## Setup

Put the setup call in your init.lua or any lua file that is sourced.

**NOTE**: A call to `.setup()` is *required* for this plugin to work!
(simrat39/symbols-outline.nvim#213)

```lua
require("symbols-outline").setup()
```

## Configuration

Pass a table to the setup call above with your configuration options.

```lua
local opts = {
  highlight_hovered_item = true,
  show_guides = true,
  auto_preview = false,
  position = 'right',
  relative_width = true,
  width = 25,
  -- Behaviour changed in this fork:
  -- Auto close the outline window if goto_location is triggered and not for
  -- focus_location
  auto_close = false,
  show_numbers = false,
  show_relative_numbers = false,
  show_symbol_details = true,
  preview_bg_highlight = 'Pmenu',
  autofold_depth = nil,
  auto_unfold_hover = true,
  fold_markers = { '', '' },
  wrap = false,
  keymaps = { -- These keymaps can be a string or a table for multiple keys
    close = {"<Esc>", "q"},
    goto_location = "<Cr>",
    focus_location = "o",
    hover_symbol = "<C-space>",
    toggle_preview = "K",
    rename_symbol = "r",
    code_actions = "a",
    fold = "h",
    fold_toggle = '<tab>',       -- Only in this fork
    fold_toggle_all = '<S-tab>', -- Only in this fork
    unfold = "l",
    fold_all = "W",
    unfold_all = "E",
    fold_reset = "R",
  },
  lsp_blacklist = {},
  symbol_blacklist = {},
  symbols = {
    -- Changed in this fork
    File = { icon = "󰈔", hl = "@text.uri" },
    Module = { icon = "󰆧", hl = "@namespace" },
    Namespace = { icon = "󰅪", hl = "@namespace" },
    Package = { icon = "󰏗", hl = "@namespace" },
    Class = { icon = "𝓒", hl = "@type" },
    Method = { icon = "ƒ", hl = "@method" },
    Property = { icon = "", hl = "@method" },
    Field = { icon = "󰆨", hl = "@field" },
    Constructor = { icon = "", hl = "@constructor" },
    Enum = { icon = "ℰ", hl = "@type" },
    Interface = { icon = "󰜰", hl = "@type" },
    Function = { icon = "", hl = "@function" },
    Variable = { icon = "", hl = "@constant" },
    Constant = { icon = "", hl = "@constant" },
    String = { icon = "𝓐", hl = "@string" },
    Number = { icon = "#", hl = "@number" },
    Boolean = { icon = "⊨", hl = "@boolean" },
    Array = { icon = "󰅪", hl = "@constant" },
    Object = { icon = "⦿", hl = "@type" },
    Key = { icon = "🔐", hl = "@type" },
    Null = { icon = "NULL", hl = "@type" },
    EnumMember = { icon = "", hl = "@field" },
    Struct = { icon = "𝓢", hl = "@type" },
    Event = { icon = "🗲", hl = "@type" },
    Operator = { icon = "+", hl = "@operator" },
    TypeParameter = { icon = "𝙏", hl = "@parameter" },
    Component = { icon = "󰅴", hl = "@function" },
    Fragment = { icon = "󰅴", hl = "@constant" },
    -- ccls
    TypeAlias =  { icon = ' ', hl = '@type' },
    Parameter = { icon = ' ', hl = '@parameter' },
    StaticMethod = { icon = ' ', hl = '@function' },
    Macro = { icon = ' ', hl = '@macro' },
  },
}
```

| Property               | Description                                                                    | Type               | Default                  |
| ---------------------- | ------------------------------------------------------------------------------ | ------------------ | ------------------------ |
| highlight_hovered_item | Whether to highlight the currently hovered symbol (high cpu usage)             | boolean            | true                     |
| show_guides            | Whether to show outline guides                                                 | boolean            | true                     |
| position               | Where to open the split window                                                 | 'right' or 'left'  | 'right'                  |
| relative_width         | Whether width of window is set relative to existing windows                    | boolean            | true                     |
| width                  | Width of window (as a % or columns based on `relative_width`)                  | int                | 25                       |
| auto_close             | Whether to automatically close the window after `goto_location`                | boolean            | false                    |
| auto_preview           | Show a preview of the code on hover                                            | boolean            | false                    |
| show_numbers           | Shows numbers with the outline                                                 | boolean            | false                    |
| show_relative_numbers  | Shows relative numbers with the outline                                        | boolean            | false                    |
| show_symbol_details    | Shows extra details with the symbols (lsp dependent)                           | boolean            | true                     |
| preview_bg_highlight   | Background color of the preview window                                         | string             | Pmenu                    |
| winblend               | Pseudo-transparency of the preview window                                      | int                | 0                        |
| keymaps                | Which keys do what                                                             | table (dictionary) | [here](#default-keymaps) |
| symbols                | Icon and highlight config for symbol icons                                     | table (dictionary) | scroll up                |
| lsp_blacklist          | Which lsp clients to ignore                                                    | table (array)      | {}                       |
| symbol_blacklist       | Which symbols to ignore ([possible values](./lua/symbols-outline/symbols.lua)) | table (array)      | {}                       |
| autofold_depth         | Depth past which nodes will be folded by default                               | int                | nil                      |
| auto_unfold_hover      | Automatically unfold hovered symbol                                            | boolean            | true                     |
| fold_markers           | Markers to denote foldable symbol's status                                     | table (array)      | { '', '' }             |
| wrap                   | Whether to wrap long lines, or let them flow off the window                    | boolean            | false                    |

## Commands

| Command                | Description            |
| ---------------------- | ---------------------- |
| `:SymbolsOutline`      | Toggle symbols outline |
| `:SymbolsOutlineOpen`  | Open symbols outline   |
| `:SymbolsOutlineClose` | Close symbols outline  |

## Default keymaps

| Key        | Action                                             |
| ---------- | -------------------------------------------------- |
| Escape     | Close outline                                      |
| Enter      | Go to symbol location in code                      |
| o          | Go to symbol location in code without losing focus |
| Ctrl+Space | Hover current symbol                               |
| K          | Toggles the current symbol preview                 |
| r          | Rename symbol                                      |
| a          | Code actions                                       |
| h          | fold symbol                                        |
| tab        | toggle fold under cursor                           |
| shift+tab  | toggle all folds                                   |
| l          | Unfold symbol                                      |
| W          | Fold all symbols                                   |
| E          | Unfold all symbols                                 |
| R          | Reset all folding                                  |
| ?          | Show help message                                  |

## Highlights

| Highlight               | Purpose                                |
| ----------------------- | -------------------------------------- |
| FocusedSymbol           | Highlight of the focused symbol        |
| Pmenu                   | Highlight of the preview popup windows |
| SymbolsOutlineConnector | Highlight of the table connectors      |
| Comment                 | Highlight of the info virtual text     |
