配置 Vim 的基础编辑体验，字体使用 CaskaydiaMono Nerd Font 配合终端 [[Alacritty 终端]]。

---

## 一、基础设置

| 设置 | 说明 |
|---|---|
| `nocompatible` | 关闭 vi 兼容 |
| `syntax on` | 语法高亮 |
| `number` | 绝对行号 |
| `relativenumber` | 相对行号（便于跳转） |
| `mouse=a` | 全模式鼠标支持 |
| `encoding=utf-8` | UTF-8 编码 |
| `t_Co=256` | 256 色支持 |

---

## 二、缩进规则

```vim
filetype indent on
set autoindent

autocmd FileType html,css,javascript setlocal tabstop=2 shiftwidth=2 expandtab
autocmd FileType python setlocal tabstop=4 shiftwidth=4 expandtab
```

- 默认：跟随文件类型
- Web 前端（HTML/CSS/JS）：2 空格
- Python：4 空格

---

## 三、搜索

| 设置 | 说明 |
|---|---|
| `hlsearch` | 高亮所有匹配 |
| `incsearch` | 增量搜索（输入时即时跳转） |
| `smartcase` | 全小写时忽略大小写，有大写时区分 |

---

## 四、界面

| 设置 | 说明 |
|---|---|
| `cursorline` | 高亮当前行 |
| `wrap` + `linebreak` | 软换行（不在单词中间断开） |
| `ruler` | 右下角显示行列号 |
| `showmode` | 显示当前模式 |
| `showcmd` | 显示正在输入的命令 |
| `showmatch` | 匹配括号高亮 |
| `wildmenu` | 命令行补全增强 |

---

## 五、与 Neovim 对比

日常主要使用 [[Neovim 配置]]（通过 `export EDITOR=nvim`），此 Vim 配置作为后备/轻量编辑场景使用。

---

## 六、配置文件

`~/.vimrc`
