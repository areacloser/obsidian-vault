使用 Neovim + lazy.nvim 插件管理器，作为主力编辑器（`export EDITOR=nvim`）。

---

## 一、目录结构

```
~/.config/nvim/
├── init.lua          # 入口文件
├── lazy-lock.json    # 插件版本锁
└── lua/
    ├── config/
    │   ├── style.lua # 编辑器基础设置
    │   └── lazy.lua  # lazy.nvim 启动配置
    └── plugins/      # 插件定义
```

---

## 二、基础编辑器设置（style.lua）

```lua
vim.g.loaded_netrw = 1          -- 禁用内置 netrw
vim.g.loaded_netrwPlugin = 1
vim.opt.termguicolors = true    -- 真彩色
vim.opt.number = true           -- 行号
vim.opt.relativenumber = true   -- 相对行号
vim.opt.cursorline = true       -- 高亮当前行
```

---

## 三、插件管理（lazy.nvim）

- 插件源：GitHub
- 安装时配色：habamax
- 自动检查更新：已启用
- 插件列表在 `lua/plugins/` 目录下
- Leader 键：空格（`mapleader = " "`）
- 本地 Leader：反斜杠

---

## 四、与 Vim 的关系

日常使用 Neovim，[[Vim 编辑器配置]] 作为后备。

---

## 五、配置文件

- `~/.config/nvim/init.lua`
- `~/.config/nvim/lua/config/style.lua`
- `~/.config/nvim/lua/config/lazy.lua`
- `~/.config/nvim/lua/plugins/`
