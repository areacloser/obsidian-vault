Wlogout 是 Wayland 下的会话注销/电源菜单，在 [[Niri 窗口管理器]] 中通过点击 [[Waybar 状态栏]] 的 Arch 图标触发。

---

## 一、菜单选项

| 标签 | 操作 | 快捷键 |
|---|---|---|
| Lock | `loginctl lock-session` | `L` |
| Hibernate | `systemctl nvidia-hibernate` | `H` |
| Logout | `loginctl terminate-user $USER` | `E` |
| Shutdown | `systemctl poweroff` | `S` |
| Suspend | `systemctl nvidia-suspend` | `U` |
| Reboot | `systemctl reboot` | `R` |

---

## 二、休眠与挂起

使用 NVIDIA 专用 systemd 服务（[[NVIDIA 驱动配置]]）而非标准 `systemctl suspend/hibernate`，确保 GPU 状态正确保存和恢复：

- `nvidia-suspend` — 挂起到内存
- `nvidia-hibernate` — 休眠到磁盘（swap 分区）

---

## 三、样式（Nordic 暗色）

- 背景：`rgba(59, 66, 82, 0.9)` — Nordic Polar Night
- 按键背景：`#1E1E1E`
- 按键悬停/选中：`#81a1c1`（Frost 蓝）
- 图标：系统默认 wlogout 图标（lock/logout/suspend/hibernate/shutdown/reboot）

---

## 四、触发方式

在 [[Waybar 状态栏]] 中：
```json
"custom/arch": {
    "format": "󰣇",
    "on-click": "wlogout"
}
```

---

## 五、配置文件

- `~/.config/wlogout/layout` — 按钮定义
- `~/.config/wlogout/style.css` — 样式
