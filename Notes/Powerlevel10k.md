Powerlevel10k（p10k）是 Zsh 的高性能提示符主题，基于 Powerlevel9k 重写，速度提升 10-100 倍。

当前配置于 2026-06-15 通过 `p10k configure` 向导生成。

---

## 一、基本配置

```bash
POWERLEVEL9K_MODE=nerdfont-v3   # 使用 Nerd Font v3 图标
POWERLEVEL9K_ICON_PADDING=moderate
POWERLEVEL9K_ICON_BEFORE_CONTENT=   # 左图标前、右图标后
POWERLEVEL9K_PROMPT_ADD_NEWLINE=true  # 提示符前空一行
```

---

## 二、左提示符布局

```
╭─  ~/Documents/Obsidian  master
╰─ ❯
```

段（Line 1）：`os_icon` → `dir` → `vcs`
段（Line 2）：`newline`（prompt_char 已注释，使用 os_icon 自带的 ❯）

### 目录显示

- 缩短策略：`truncate_to_unique`（缩到最短唯一前缀）
- 最大长度：80 列
- 锚目录（含 `.git` 等标记的目录）加粗蓝色
- 通常目录青色，缩短部分灰色
- 非可写目录显示锁图标

### Git 状态

自定义 `my_git_formatter`，显示：
- 分支名（>32字符截断）
- 远程跟踪状态（⇣落后 ⇡领先）
- stash 数量（`*`）
- 暂存/未暂存/冲突/未跟踪计数

---

## 三、右提示符布局

包含大量语言版本管理段（asdf、pyenv、nvm、rbenv 等），实际只在有对应环境时显示。

常用显示的段：
- `status` — 上条命令退出码（✔/✘）
- `command_execution_time` — 超过 3 秒显示耗时
- `background_jobs` — 后台任务指示
- `context` — SSH 或 root 时显示 user@hostname
- `time` — 当前时间

---

## 四、关键链接符号

```
╭─  →  MULTILINE_FIRST_PROMPT_PREFIX
├─  →  MULTILINE_NEWLINE_PROMPT_PREFIX
╰─  →  MULTILINE_LAST_PROMPT_PREFIX
```

段间分隔符使用 Powerline 三角符号（   ）。

---

## 五、性能特性

- **Instant prompt**：在 Zsh 加载插件前就显示提示符，感知上瞬间启动
- **gitstatus**：异步 Git 状态检测，不阻塞输入
- **transient prompt**：执行命令后提示符自动精简，减少滚动

---

## 六、配置文件

- 主配置：`~/.p10k.zsh`
- 在 `~/.zshrc` 末尾通过 `[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh` 加载
