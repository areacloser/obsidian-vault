Fcitx5 是 Linux 下新一代输入法框架，Wayland 原生支持。配置使用 Rime 引擎 + 雾凇拼音（rime-ice）。

---

## 一、安装的包

```bash
sudo pacman -S fcitx5 fcitx5-chinese-addons fcitx5-qt fcitx5-gtk \
    fcitx5-configtool fcitx5-pinyin-zhwiki \
    fcitx5-rime rime-ice
```

| 包                          | 用途           |
| -------------------------- | ------------ |
| `fcitx5`                   | 输入法核心        |
| `fcitx5-chinese-addons`    | 中文附加组件（拼音表等） |
| `fcitx5-qt` / `fcitx5-gtk` | Qt/GTK 输入法模块 |
| `fcitx5-configtool`        | 图形配置工具       |
| `fcitx5-pinyin-zhwiki`     | 中文维基百科词库     |
| `fcitx5-rime`              | Rime 输入法引擎   |
| `rime-ice`                 | 雾凇拼音方案       |

---

## 二、环境变量

（在 [[Niri 窗口管理器]] 中设置）

```kdl
QT_IM_MODULE "fcitx"
XMODIFIERS "@im=fcitx"
SDL_IM_MODULE "fcitx"
```

---

## 三、自启动

在 [[Niri 窗口管理器]] misc.kdl 中：
```kdl
spawn-at-startup "fcitx5" "-d"
```

---

## 四、Rime 雾凇拼音配置

`~/.local/share/fcitx5/rime/default.custom.yaml`：
```yaml
patch:
  schema_list:
    - schema: ice_pinyin
```

仅启用雾凇拼音方案，简洁实用。

---

## 五、相关笔记

- [[Niri 窗口管理器]] — Wayland 合成器
- [[GTK 主题配置]] — 桌面主题一致性
