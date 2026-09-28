Fzf 是命令行模糊搜索工具，在 [[Zsh 配置]] 中深度集成。

---

## 一、功能

在 Zsh 中提供的快捷键：

| 快捷键 | 功能 |
|---|---|
| `Ctrl+T` | 模糊搜索当前目录文件/目录，选中后粘贴路径到命令行 |
| `Ctrl+R` | 模糊搜索命令历史（替代原生 Ctrl+R） |
| `Alt+C` | 模糊搜索目录并 cd |

---

## 二、Zsh 集成

```bash
source <(fzf --zsh)  # 加载原生 Zsh 快捷键绑定
```

插件 `fzf-zsh-plugin` 和 `fzf-tab` 提供增强 Tab 补全功能。

---

## 三、安装

```bash
sudo pacman -S fzf
```

`~/.fzf.zsh` 和 `~/.fzf/` 目录保存本地配置。

---

## 四、相关笔记

- [[Zsh 配置]] — Zsh 集成
- [[Neovim 配置]] — 可在 Neovim 中通过 Telescope 等插件模糊搜索
