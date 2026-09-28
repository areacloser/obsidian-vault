Waybar 是 Wayland 下的高度可定制的状态栏，本配置采用 Nordic 暗色主题，专为 [[Niri 窗口管理器]] 设计。

---

## 一、布局结构

```
[] [1 2 3 4 5 6] [taskbar]  |  [window title]  |  [tray] [net · vol · cpu · mem · bat] [clock]
  左端                          中间                 右端
```

- 位置：屏幕顶部，高 32px
- 字体：`LXGW WenKai Mono`, 13px
- 配色：Nordic Polar Night 色板（[[GTK 主题配置]]）

---

## 二、模块详情

### 左端

| 模块 | 配置 |
|---|---|
| `custom/arch` | Arch 图标 󰣇，点击打开 [[Wlogout 注销菜单]] |
| `niri/workspaces` | 6 个固定工作区，图标分别表示终端/浏览器/笔记/通讯/MC/音乐，隐藏空工作区 |
| `wlr/taskbar` | 窗口任务栏，Papirus-Dark 图标主题，Steam 游戏统一显示为 "Game" |
| `tray` | 系统托盘（蓝牙等） |

### 中间

| 模块 | 配置 |
|---|---|
| `niri/window` | 当前窗口标题，含正则重写规则（如 Firefox 标题添加 󰖟 图标、vim/nvim 显示编辑器图标） |

### 右端（合并胶囊）

| 模块 | 图标 | 颜色 |
|---|---|---|
| `network` | / 上下行速度 | Frost 蓝 #88C0D0 |
| `pulseaudio` | 音量百分比，蓝牙显示  | Aurora 绿 #A3BE8C |
| `cpu` |  使用率 | Aurora 黄 #EBCB8B |
| `memory` |  百分比 | Aurora 紫 #B48EAD |
| `battery` | 电池图标+百分比 | 绿色 → 黄色警告 → 红色闪烁 |
| `clock` |  HH:MM，右击切换日期 | Snow Storm 白 #E5E9F0 |

---

## 三、样式要点（Nordic 暗色）

- 底色：`#3B4252`
- 悬停：`#434C5E`
- 活跃：`#5E81AC`
- 文字：`#D8DEE9`
- 模块间距：3-6px
- 圆角：10px

右端 network → battery 做成分段胶囊（无圆角间隙），clock 单独胶囊。

---

## 四、颜色语义

- 警告：`#D08770`（橙）
- 危急：`#BF616A`（红）
- 断开/静音：`#4C566A`（灰暗）

---

## 五、配置文件

- `~/.config/waybar/config.jsonc` — 模块布局与行为
- `~/.config/waybar/style.css` — 样式（Nordic 主题）
