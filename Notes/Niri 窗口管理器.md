Niri 是 Wayland 下的可滚动平铺式合成器（scrollable-tiling compositor），工作区在水平方向无限延伸，类似 PaperWM 的滚动体验。使用 KDL 配置格式。

---

## 一、配置文件结构

配置拆分为多文件，通过 `include` 组合（`~/.config/niri/config.kdl`）：

| 文件 | 内容 |
|---|---|
| `misc.kdl` | 环境变量、自启动、动画 |
| `workspaces.kdl` | 工作区定义 |
| `input.kdl` | 键盘/触控板/鼠标/光标 |
| `outputs.kdl` | 显示器设置 |
| `layout.kdl` | 布局、间隙、焦点环 |
| `window-rules.kdl` | 窗口匹配规则 |
| `layer-rules.kdl` | 图层匹配规则 |
| `binds.kdl` | 快捷键 |

---

## 二、显示器配置

```
output "eDP-1"
  mode "2560x1600@60.002"
  scale 1.5
```

笔记本内置屏幕，2560×1600 分辨率，1.5 倍缩放（逻辑分辨率约 1707×1067）。

---

## 三、工作区

固定 6 个工作区（垂直排列），在 [[Waybar 状态栏]] 中对应 6 个图标。

| 工作区 | 用途            | Waybar 图标 |
| --- | ------------- | --------- |
| 1   | 终端（Alacritty） |          |
| 2   | 浏览器（Firefox）  |          |
| 3   | 笔记（Obsidian）  |          |
| 4   | 通讯（QQ/微信）     |          |
| 5   | 启动器（HMCL）     |          |
| 6   | 媒体            |          |

---

## 四、窗口规则

### 自动放置

- **Alacritty** → 工作区 1，自动最大化，背景模糊
- **Firefox** → 工作区 2，透明度 0.95，自动最大化
- **Obsidian** → 工作区 3，自动最大化
- **QQ / WeChat** → 工作区 4
- **HMCL** → 工作区 5
- **Minecraft** → 浮动窗口

### 全局圆角

所有窗口应用 `geometry-corner-radius 10` + `clip-to-geometry true`。

### Firefox 画中画

匹配 Firefox 的 Picture-in-Picture 窗口，自动设为浮动。

---

## 五、快捷键

基础修饰键：`Mod` = Super（Tty）/ Alt（winit）

| 快捷键                | 功能                      |
| ------------------ | ----------------------- |
| `Mod+Return`       | 打开 Alacritty            |
| `Mod+D`            | 打开 Fuzzel 启动器           |
| `Mod+Q`            | 关闭窗口                    |
| `Mod+O`            | 概览模式                    |
| `Mod+H/J/K/L`      | 聚焦窗口（左/下/上/右）           |
| `Mod+Ctrl+H/J/K/L` | 移动窗口                    |
| `Mod+1-9`          | 切换到工作区 1-9              |
| `Mod+Ctrl+1-9`     | 移动窗口到工作区                |
| `Mod+V`            | 切换浮动/平铺                 |
| `Mod+W`            | 切换标签页模式                 |
| `Mod+F`            | 最大化列                    |
| `Mod+Shift+F`      | 全屏窗口                    |
| `Mod+R`            | 循环列宽预设（1/3 → 1/2 → 2/3） |
| `Mod+P`            | 截屏（选择区域）                |
| `Mod+Insert`       | 截屏（整个屏幕）                |
| `Mod+Shift+Insert` | 截屏（当前窗口）                |
| `Mod+Shift+E`      | 退出 Niri                 |
| `Mod+Shift+P`      | 关闭显示器                   |
| `Mod+Escape`       | 解除键盘快捷键锁定               |
| `Mod+B`            | 重载 Waybar               |
| `Super+Alt+L`      | 锁屏                      |

### 媒体键

- `XF86AudioRaiseVolume` / `XF86AudioLowerVolume` — 音量 ±10%
- `XF86AudioMute` — 静音切换
- `XF86AudioMicMute` — 麦克风静音
- `XF86MonBrightnessUp` / `XF86MonBrightnessDown` — 亮度 ±10%
- `XF86AudioPlay/Stop/Prev/Next` — 媒体播放控制（playerctl）

---

## 六、输入设备

- 键盘：Systemd-locale1 管理 xkb
- 触控板：点击（tap）、自然滚动、双指滚动
- 光标：Bibata-Modern-Ice 主题，大小 24，打字时不隐藏

---

## 七、自启动

```
waybar                          # 状态栏
fcitx5 -d                       # 输入法
mako                            # 通知守护进程
swaybg -i <wallpaper> -m fill   # 静态壁纸
awww-daemon; awww img <image>   # 动态壁纸（覆盖 swaybg）
```

---

## 八、环境变量

```kdl
QT_IM_MODULE "fcitx"
XMODIFIERS "@im=fcitx"
SDL_IM_MODULE "fcitx"
QT_QPA_PLATFORM "wayland"
GDK_BACKEND "wayland"
```

---

## 九、布局配置

- 窗口间隙：16 逻辑像素
- 默认列宽：50%
- 预设列宽：1/3、1/2、2/3
- 焦点环：3px 宽度，渐变（`#80c8ff` → `#c7ff7f`），45°
- 模糊：3 passes，偏移 3.0，噪声 0.05

---

## 十、相关笔记

- [[Waybar 状态栏]] — 配套状态栏
- [[Fuzzel 启动器]] — 应用启动器
- [[Mako 通知守护进程]] — 通知管理
- [[Alacritty 终端]] — 默认终端
