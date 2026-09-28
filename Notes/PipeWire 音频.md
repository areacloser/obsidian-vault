PipeWire 是 Linux 新一代音视频框架，替代 PulseAudio 和 JACK。在 [[Niri 窗口管理器]] 中作为默认音频服务。

---

## 一、安装的包

```bash
sudo pacman -S pipewire pipewire-pulse wireplumber
```

| 包 | 用途 |
|---|---|
| `pipewire` | 核心音频/视频处理 |
| `pipewire-pulse` | PulseAudio 兼容层（传统应用无需修改） |
| `wireplumber` | 会话管理器（替代 pipewire-media-session） |

---

## 二、用户服务

```bash
systemctl --user enable pipewire pipewire-pulse wireplumber
```

---

## 三、音量控制

在 [[Niri 窗口管理器]] 通过 wpctl 控制：

| 快捷键 | 操作 |
|---|---|
| `XF86AudioRaiseVolume` | `wpctl set-volume @DEFAULT_AUDIO_SINK@ 0.1+ -l 1.0` |
| `XF86AudioLowerVolume` | `wpctl set-volume @DEFAULT_AUDIO_SINK@ 0.1-` |
| `XF86AudioMute` | `wpctl set-mute @DEFAULT_AUDIO_SINK@ toggle` |
| `XF86AudioMicMute` | `wpctl set-mute @DEFAULT_AUDIO_SOURCE@ toggle` |

音量上限锁定 100%。

---

## 四、Waybar 集成

[[Waybar 状态栏]] 中通过 `pulseaudio` 模块显示音量、静音状态和在蓝牙设备上显示图标。

---

## 五、相关笔记

- [[Niri 窗口管理器]] — Wayland 合成器
- [[Waybar 状态栏]] — 音量显示
