Btop 是终端资源监视器（C++ 重写的 bpytop/bashtop），在 [[Alacritty 终端]] 中运行。

---

## 一、配置要点

| 设置 | 值 |
|---|---|
| 色彩主题 | Default |
| 真彩色 | 是 |
| 终端背景 | 不透明（`theme_background = true`） |
| 圆角 | 是 |
| Vim 按键 | 否 |
| 鼠标 | 启用 |

---

## 二、预设布局

三组预设可通过 `1`/`2`/`3` 键切换：

```
1: CPU(alt) + 进程(default)
2: CPU(default) + 内存(default) + 网络(default)
3: CPU(block) + 网络(tty)
```

---

## 三、配置文件

`~/.config/btop/btop.conf`
