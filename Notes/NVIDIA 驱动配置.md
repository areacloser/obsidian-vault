在 Arch Linux 上为 RTX 5060 Laptop GPU 配置 NVIDIA 开源驱动，并启用 Wayland 休眠支持。

---

## 一、硬件信息

- GPU：NVIDIA RTX 5060 Laptop
- 笔记本：华硕天选6 Pro 锐龙版
- CPU：AMD Ryzen 9 9955HX（带集成显卡，但使用独显输出）
- 内核：linux-zen

---

## 二、驱动安装

```bash
sudo pacman -S nvidia-open-dkms nvidia-utils lib32-nvidia-utils nvidia-settings
```

选择开源内核模块（`nvidia-open-dkms`），通过 DKMS 随内核自动编译。

---

## 三、屏蔽 Nouveau

（`/etc/modprobe.d/blacklist-nouveau.conf`）：
```
blacklist nouveau
options nouveau modeset=0
```

防止开源驱动冲突。

---

## 四、内核参数

在 [[Arch Linux & Windows 双系统安装]] 中通过 GRUB 配置：

```
nvidia_drm.modeset=1
nvidia.NVreg_PreserveVideoMemoryAllocations=1
```

- `modeset=1`：启用 DRM 模式设置（Wayland 必须）
- `PreserveVideoMemoryAllocations=1`：配合休眠保存显存

---

## 五、休眠服务

```bash
sudo systemctl enable nvidia-suspend nvidia-resume nvidia-hibernate
```

与 [[Wlogout 注销菜单]] 中的休眠/挂起按钮配合使用：
- `systemctl nvidia-suspend` — 挂起到内存
- `systemctl nvidia-hibernate` — 休眠到磁盘

---

## 六、验证

```bash
nvidia-smi            # 查看 GPU 状态
lsmod | grep nvidia   # 确认内核模块加载
```

---

## 七、相关笔记

- [[Arch Linux & Windows 双系统安装]] — 系统安装
- [[Wlogout 注销菜单]] — 休眠/挂起操作
- [[Btrfs文件系统]] — 独立的 swap 分区支持休眠
