В neovim есть специальная глобальная переменная `vim`. С её помощью можно настраивать различные вещи. Например:
-   `vim.api`: можно получить доступ к различным функциям api (например, `vim.api.nvim_get_curren_line()`)
-   `vim.ui`: изменяет UI параметры.
-   `vim.lsp`: контролирует встроенный LSP клиент *(LSP = Language Server Provider)*

### Vimscript из lua
Можно вызвать при помощи `vim.api.nvim_command()`.
Например: `vim.api.nvim_command(set nonumber)`

Также можно выставлять какие-либо опции при помощи `vim.api.nvim_set_option()`.
Например: `vim.api.nvim_set_option('smarttab', false)`

Также опции можно выставлять при помощи помощи `vim.opt`.
Например: `vim.opt.smarttab = false`