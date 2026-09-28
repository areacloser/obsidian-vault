统一桌面外观，采用 Nordic 暗色主题 + Papirus 图标 + 中文等宽字体。

---

## 一、主题配置

### GTK 3/4（`~/.config/gtk-3.0/settings.ini`）

| 设置项 | 值 |
|---|---|
| 主题 | Nordic |
| 图标 | Papirus-Dark |
| 字体 | CaskaydiaMono Nerd Font Semi-Light 11 |
| 光标 | Bibata-Modern-Ice, 24px |
| 偏好暗色 | 是 |
| 抗锯齿 | 启用 |
| 微调 | 轻微（hintslight） |
| 子像素 | RGB |

### xsettingsd（`~/.config/xsettingsd/xsettingsd.conf`）

为 XWayland 应用提供相同的主题设置（Nordic / Papirus-Dark / Bibata-Modern-Ice）。

---

## 二、图标主题

- Papirus-Dark：Waybar 任务栏图标、文件管理器图标
- Bibata-Modern-Ice：光标主题（[[Niri 窗口管理器]] 中同样配置）

---

## 三、字体配置

系统字体配置（`~/.config/fontconfig/fonts.conf`）确保中英文混排使用合适的回退字体。

已安装的主要字体（详见 [[安装中文字体]]）：
- Noto Sans/Serif CJK
- Sarasa Gothic（更纱黑体）
- LXGW WenKai（霞鹜文楷，[[Waybar 状态栏]]、[[Fuzzel 启动器]]、[[Mako 通知守护进程]] 中使用）
- 思源黑体/宋体

---

## 四、与各组件的关系

- [[Waybar 状态栏]] — 使用 Nordic 色板参数
- [[Alacritty 终端]] — 使用 Nordic 终端主题
- [[Wlogout 注销菜单]] — 使用 Nordic 底色
- [[Niri 窗口管理器]] — 窗口圆角、模糊与整体视觉集成

---

## 五、配置文件

- `~/.config/gtk-3.0/settings.ini`
- `~/.config/gtk-4.0/settings.ini`
- `~/.config/xsettingsd/xsettingsd.conf`
- `~/.config/fontconfig/fonts.conf`
- `~/.gtkrc-2.0`
