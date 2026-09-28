## 一、配置文件

`~/.config/rog/rog-control-center.cfg`（RON 格式）

```ron
(
    run_in_background: true,
    startup_in_background: false,
    enable_tray_icon: true,
    dark_mode: true,
    notifications: (
        enabled: true,
        receive_notify_gfx: true,
        receive_notify_gfx_status: true,
    ),
)
```

---

## 二、功能

- 后台运行、系统托盘图标
- 暗色模式
- 通知启用（含 GPU 状态通知）
- 管理笔记本电源模式、风扇曲线、键盘灯光等

---

## 三、硬件支持

华硕天选6 Pro 锐龙版（AMD Ryzen 9 9955HX + RTX 5060），rog-control-center 提供华硕笔记本的硬件控制面板。

---

## 四、相关笔记

- [[NVIDIA 驱动配置]] — GPU 驱动
