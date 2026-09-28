Ly 是 Linux TUI 显示管理器，轻量、美观，不支持 Wayland 直接渲染（在 TTY 中运行），通过 systemd 管理会话。

---

## 一、安装与启用

```bash
sudo pacman -S ly
sudo systemctl enable ly
```

替换了默认的 GDM/SDDM，减少依赖和资源占用。

---

## 二、工作原理

- Ly 在 TTY 中运行，用户登录后启动 Wayland 会话（[[Niri 窗口管理器]]）
- 会话选择：通过 `/usr/share/wayland-sessions/niri.desktop` 自动识别

---

## 三、相关笔记

- [[Arch Linux & Windows 双系统安装]] — 系统安装流程
- [[Niri 窗口管理器]] — 登录后启动的 Wayland 合成器
