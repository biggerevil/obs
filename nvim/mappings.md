## Первый способ (vim.api.nvim_set_keyamp)
Для установки mapping'а используется `vim.api.nvim_set_keymap()`.
Примеры:
- `vim.api.nvim_set_keymap('n', '<Leader><Space>', ':set hlsearch!<CR>', { noremap = true, silent = true })`
- `vim.api.nvim_set_keymap('n', '<Leader>tegf',  [[<Cmd>lua require('telescope.builtin').git_files()<CR>]], { noremap = true, silent = true })`
Первый аргумент: имя режима, в котором должен работать mapping. См. таблицу ниже.
Второй аргумент: при нажатии каких клавиш команда будет исполнена.
Третий аргумент: какая команда будет исполнена.
Последний аргумент: noremap и silent. `noremap`, **кажется**, отвечает, будет ли исполнена команда будучи только в конкретном режиме. А `silent` показывает, будет ли выведен какой-либо текст (кажется, по умолчанию это внизу слева).

Таблица режимов для указания первым аргументом в `vim.api.nvim_set_keymap`.
| String value           | Help page     | Affected modes                           | Vimscript equivalent |
| ---------------------- | ------------- | ---------------------------------------- | -------------------- |
| `''` (an empty string) | `mapmode-nvo` | Normal, Visual, Select, Operator-pending | `:map`               |
| `'n'`                  | `mapmode-n`   | Normal                                   | `:nmap`              |
| `'v'`                  | `mapmode-v`   | Visual and Select                        | `:vmap`              |
| `'s'`                  | `mapmode-s`   | Select                                   | `:smap`              |
| `'x'`                  | `mapmode-x`   | Visual                                   | `:xmap`              |
| `'o'`                  | `mapmode-o`   | Operator-pending                         | `:omap`              |
| `'!'`                  | `mapmode-ic`  | Insert and Command-line                  | `:map!`              |
| `'i'`                  | `mapmode-i`   | Insert                                   | `:imap`              |
| `'l'`                  | `mapmode-l`   | Insert, Command-line, Lang-Arg           | `:lmap`              |
| `'c'`                  | `mapmode-c`   | Command-line                             | `:cmap`              |
| `'t'`                  | `mapmode-t`   | Terminal                                 | `:tmap`              |

## Второй способ (vim.keymap.set)
Доступен только с nvim версии 0.7.0+ .
Для подробного описания параметров см. первый способ.
Примеры:
`vim.keymap.set('n', '<Leader>ex1', '<Cmd>lua vim.notify("Example 1")<CR>')e`
`vim.keymap.set({'n', 'c'}, '<Leader>ex2', '<Cmd>lua vim.notify("Example 2")<CR>')`
Также можно передавать последний параметр с, например, такими значениями:
`vim.keymap.set('n', '<Leader>ex1', '<Cmd>echomsg "Example 1"<CR>', {buffer = true})
`vim.keymap.set('n', '<Leader>ex2', function() print('Example 2') end, {desc = 'Prints "Example 2" to the message area'})`

