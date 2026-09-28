Zsh 是 Arch Linux 的默认交互 shell，本笔记记录基于 [[Oh My Zsh]] + [[Powerlevel10k]] 的配置方案。

---

## 一、框架与插件

### Oh My Zsh

安装路径：`~/.oh-my-zsh`
Tty 终端使用 `frisk` 主题，图形终端使用 [[Powerlevel10k]]。

已启用的插件（`~/.zshrc`）：

| 插件                        | 功能                                     |
| ------------------------- | -------------------------------------- |
| `git`                     | Git 别名与分支信息                            |
| `z`                       | 基于频率的目录跳转（`~/.z` 文件记录）                 |
| `extract`                 | 通用解压命令（`extract <file>` 自动识别格式）        |
| `zsh-autosuggestions`     | 灰色历史建议，按 → 接受                          |
| `zsh-syntax-highlighting` | 命令语法高亮                                 |
| `fzf-zsh-plugin`          | [[Fzf 模糊搜索]] 集成（Ctrl+T 搜文件，Ctrl+R 搜历史） |
| `fzf-tab`                 | Tab 补全时用 fzf 交互式选择                     |

### fzf 集成

```bash
source <(fzf --zsh)   # 加载 fzf 原生 zsh 绑定
```

快捷键：
- `Ctrl+T` — 模糊搜索当前目录文件并粘贴路径
- `Ctrl+R` — 模糊搜索命令历史
- `Alt+C` — 模糊搜索目录并 cd

### 编辑器

```bash
export EDITOR=nvim
```

---

## 二、Powerlevel10k 提示符

配置详见 [[Powerlevel10k]]。

要点：
- 模式：nerdfont-v3 + powerline
- 样式：classic、angled separators、sharp heads、round tails
- 布局：双行，左帧
- 即时提示（instant prompt）已启用

重新配置：`p10k configure`

---

## 三、自定义别名

```bash
alias ff=fastfetch   # 快速查看系统信息
```

其他别名由 Oh My Zsh 插件提供（`git` 插件提供 `gst`、`gco`、`gcb` 等常用 Git 别名）。

---

## 四、相关配置

- `~/.zshrc` — 主配置文件
- `~/.p10k.zsh` — Powerlevel10k 主题配置
- `~/.oh-my-zsh/` — Oh My Zsh 安装目录
- `~/.z` — z 插件数据库
