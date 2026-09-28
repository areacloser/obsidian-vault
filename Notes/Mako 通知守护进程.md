Mako 是 Wayland 下的轻量级通知守护进程，在 [[Niri 窗口管理器]] 启动时自动运行。

---

## 一、配置

- 位置：右上角
- 排序：最新在上（`sort=-time`）
- 层级：overlay（覆盖在窗口之上）
- 尺寸：宽 450px、高 150px
- 超时：默认 5 秒（紧急通知不自动关闭）
- 图标：最大 64px
- 字体：LXGW WenKai 14px
- 间距：外边距 12px、内边距 12px 横向 20px 纵向
- 圆角：12px
- 边框：2px，带半透明背景 `#22222266`

---

## 二、紧急程度配色

| 级别 | 边框颜色 |
|---|---|
| low（低） | `#aaaaaa` 灰 |
| normal（普通） | `#cceeff` 浅蓝 |
| critical（紧急） | `#ff7777` 红（不自动关闭） |

---

## 三、自启动

在 [[Niri 窗口管理器]] 中配置：
```kdl
spawn-at-startup "mako"
```

---

## 四、配置文件

`~/.config/mako/config`
