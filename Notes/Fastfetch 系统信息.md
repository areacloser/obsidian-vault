Fastfetch 是 neofetch 的 C 语言继任者，速度更快、维护更活跃。在 [[Zsh 配置]] 中通过 `alias ff=fastfetch` 快速调用。

---

## 一、配置概览

配置受 Catnap 风格启发，采用框线装饰布局，`#keys` 变量带颜色标注。

```
╭───────────╮
│  user    │
│ 󰇅 host    │
│ 󰅐 uptime  │
├───────────┤
│  distro  │
│  kernel  │
│  package │
├───────────┤
│  wm      │
│ 󰇄 desktop │
│  term    │
│  shell   │
├───────────┤
│ 󰍛 cpu     │
│ 󰇞 gpu     │
│ 󰉉 disk    │
│  memory  │
├───────────┤
│  colors  │
╰───────────╯
```

---

## 二、模块详情

- Logo：自动检测发行版
- 分隔符：空格
- 颜色块：圆形符号
- CPU：显示性能核/效率核数量
- 磁盘：仅显示根分区

---

## 三、颜色数字对照

配置中使用 `{#NN}` 格式的颜色代码：

| 编号 | 对应模块 |
|---|---|
| 31 | user、term |
| 32 | host、shell |
| 33 | uptime、cpu、gpu |
| 34 | distro、disk |
| 35 | kernel |
| 36 | wm、desktop、memory |
| 37 | package |
| 39 | colors |

---

## 四、配置文件

`~/.config/fastfetch/config.jsonc`
