Alacritty 是 GPU 加速的跨平台终端模拟器，Wayland 原生支持，作为 [[Niri 窗口管理器]] 的默认终端。

---

## 一、基本配置

- 主题：Nordic（从 `themes/nordic.toml` 导入）
- 窗口透明：0.7 不透明度
- 模糊背景：启用
- 动态内边距：启用
- 实时重载：`live_config_reload = true`

---

## 二、字体

```
normal: CaskaydiaMono Nerd Font, Regular
bold:   CaskaydiaMono Nerd Font, Bold
italic: CaskaydiaMono Nerd Font, Italic
```

使用 Nerd Font 以支持 [[Powerlevel10k]] 中的图标。

---

## 三、快捷键绑定

| 快捷键 | 功能 |
|---|---|
| `Shift+Return` | 发送 `\r`（在终端中运行命令的替代换行） |

---

## 四、Nordic 配色

| 元素 | 色值 |
|---|---|
| 背景黑 | `#242933` |
| 前景 | `#BBBDAF` |
| 选择文字 | `#000000` |
| 选择背景 | `#F0DFAF` |

---

## 五、Niri 窗口规则

在 [[Niri 窗口管理器]] 中：
- 自动在工作区 1 启动并最大化
- 启用背景模糊效果

---

## 六、配置文件

- `~/.config/alacritty/alacritty.toml` — 主配置
- `~/.config/alacritty/themes/nordic.toml` — 主题配色
