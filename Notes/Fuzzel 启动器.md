Fuzzel 是 Wayland 原生的应用启动器，类似 dmenu/rofi，在 [[Niri 窗口管理器]] 中通过 `Mod+D` 调用。

---

## 一、配置

```ini
[colors]
background=000000cc    # 半透明黑色背景

[main]
font=LXGW WenKai      # 中文字体
```

极简配置，主要使用默认行为。

---

## 二、Niri 快捷键

| 快捷键 | 功能 |
|---|---|
| `Mod+D` | 打开 Fuzzel 启动器 |

---

## 三、Niri 图层规则

- 匹配 `launcher` 命名空间
- 启用背景模糊效果

---

## 四、配置文件

`~/.config/fuzzel/fuzzel.ini`
