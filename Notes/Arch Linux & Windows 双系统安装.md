## 硬件环境
- 机型：华硕天选6 Pro 锐龙版
- CPU：AMD Ryzen 9 9955HX
- GPU：NVIDIA RTX 5060 Laptop
- 存储：Samsung 1TB NVMe SSD
- 目标：Windows 11 + Arch Linux（linux-zen内核）+ [[Btrfs文件系统]]

---

## 一、安装前准备（在 Windows 中完成）

1. **关闭快速启动**
   控制面板 → 电源选项 → 选择电源按钮的功能 → 关闭快速启动

2. **关闭 BitLocker**
   设置 → 隐私和安全性 → 设备加密 → 关闭

3. **压缩 C 盘腾出空间**
   磁盘管理 → 右键C盘 → 压缩卷，留出约700GB给Linux

4. **BIOS 设置**
   - 关闭 Secure Boot
   - 确认硬盘模式为 AHCI（非VMD）
   - 启动顺序改为 USB 优先
   - 确认为 UEFI 模式

3. **[[扩展EFI分区]]**

---

## 二、分区规划

```
/dev/nvme0n1p1   4GB    EFI System         （Windows已有，复用）
/dev/nvme0n1p2   16MB   Microsoft Reserved
/dev/nvme0n1p3   270GB  Windows OS
/dev/nvme0n1p4   700MB  Windows Recovery
/dev/nvme0n1p5   28GB   Microsoft Basic
/dev/nvme0n1p6   260MB  Windows Recovery
/dev/nvme0n1p7   16GB   Linux swap
/dev/nvme0n1p8   634GB  Linux Btrfs（根目录）
```

---

## 三、Btrfs 子卷规划

| 子卷         | 挂载点         |
| ---------- | ----------- |
| @          | /           |
| @home      | /home       |
| @var       | /var        |
| @snapshots | /.snapshots |

---

## 四、安装流程

### 1. 启动 Arch ISO，联网

```bash
iwctl
station wlan0 scan
station wlan0 get-networks
station wlan0 connect 你的WiFi名称
exit

ping -c 3 archlinux.org
```

### 2. 换源

```bash
reflector --country China --age 12 --protocol https \
    --sort rate --save /etc/pacman.d/mirrorlist
```

### 3. 格式化分区

```bash
# swap
mkswap /dev/nvme0n1p7
swapon /dev/nvme0n1p7

# Btrfs
mkfs.btrfs -f -L ArchLinux /dev/nvme0n1p8
```

### 4. 创建 Btrfs 子卷

```bash
mount /dev/nvme0n1p8 /mnt

btrfs subvolume create /mnt/@
btrfs subvolume create /mnt/@home
btrfs subvolume create /mnt/@var
btrfs subvolume create /mnt/@snapshots

umount /mnt
```

### 5. 挂载分区

```bash
# 根子卷
mount -o noatime,compress=zstd,subvol=@ /dev/nvme0n1p8 /mnt

# 创建挂载点
mkdir -p /mnt/{boot,home,var,.snapshots}

# 其余子卷
mount -o noatime,compress=zstd,subvol=@home /dev/nvme0n1p8 /mnt/home
mount -o noatime,compress=zstd,subvol=@var /dev/nvme0n1p8 /mnt/var
mount -o noatime,compress=zstd,subvol=@snapshots /dev/nvme0n1p8 /mnt/.snapshots

# EFI分区（复用Windows的，注意挂载到/mnt/boot）
mount /dev/nvme0n1p1 /mnt/boot
```

确认挂载：
```bash
findmnt | grep mnt
```

### 6. 安装基础系统

```bash
pacstrap /mnt base base-devel linux-zen linux-zen-headers linux-firmware \
    btrfs-progs grub efibootmgr os-prober networkmanager vim sudo
```

### 7. 生成 fstab

```bash
genfstab -U /mnt >> /mnt/etc/fstab
cat /mnt/etc/fstab   # 确认六条记录都在
```

### 8. chroot 基础配置

```bash
arch-chroot /mnt

# 时区
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc

# 语言
echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen
echo "zh_CN.UTF-8 UTF-8" >> /etc/locale.gen
locale-gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf

# 主机名
echo "archlinux" > /etc/hostname

# root 密码
passwd
```

### 9. 配置 mkinitcpio

```bash
vim /etc/mkinitcpio.conf
```

HOOKS 改为：
```
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole block btrfs filesystems fsck)
```

重新生成：
```bash
mkinitcpio -P
```

### 10. 安装配置 GRUB

```bash
grub-install --target=x86_64-efi \
    --efi-directory=/boot \
    --bootloader-id=GRUB \
    --recheck
```

编辑 `/etc/default/grub`：
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet resume=UUID=你的swap分区UUID nvidia_drm.modeset=1 nvidia.NVreg_PreserveVideoMemoryAllocations=1"
GRUB_DISABLE_OS_PROBER=false
```

生成配置：
```bash
grub-mkconfig -o /boot/grub/grub.cfg
# 确认输出有 Found linux image 和 Found Windows Boot Manager
```

### 11. 创建用户

```bash
useradd -m -G wheel -s /bin/bash 用户名
passwd 用户名

# 开启sudo
EDITOR=vim visudo
# 去掉 %wheel ALL=(ALL:ALL) ALL 的注释
```

### 12. 启用服务并退出

```bash
systemctl enable NetworkManager
exit
umount -R /mnt
reboot
```

---

## 五、首次进入系统后的配置

### 1. 联网

```bash
nmcli device wifi connect WiFi名称 password 密码
```

### 2. 添加仓库（/etc/pacman.conf）

```
# 启用 multilib（去掉注释）
[multilib]
Include = /etc/pacman.d/mirrorlist

# 末尾添加
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch

[arch4edu]
Server = https://mirrors.tuna.tsinghua.edu.cn/arch4edu/$arch
```

导入密钥：
```bash
sudo pacman -Sy archlinuxcn-keyring
sudo pacman-key --recv-keys 7931B6D628C8D3BA
sudo pacman-key --lsign-key 7931B6D628C8D3BA
sudo pacman -Syyu
```

### 3. 安装 NVIDIA 驱动

```bash
sudo pacman -S nvidia-open-dkms nvidia-utils lib32-nvidia-utils nvidia-settings
```

屏蔽 nouveau（`/etc/modprobe.d/blacklist-nouveau.conf`）：
```
blacklist nouveau
options nouveau modeset=0
```

启用休眠服务：
```bash
sudo systemctl enable nvidia-suspend nvidia-resume nvidia-hibernate
```

### 4. 安装桌面环境（niri）

```bash
sudo pacman -S niri xwayland-satellite \
    waybar fuzzel swaylock swayidle \
    alacritty wl-clipboard \
    xdg-desktop-portal-gnome \
    pipewire pipewire-pulse wireplumber \
    qt5-wayland qt6-wayland

systemctl --user enable pipewire pipewire-pulse wireplumber

# 复制默认配置
mkdir -p ~/.config/niri
cp /usr/share/doc/niri/example-config.kdl ~/.config/niri/config.kdl
```

### 5. 安装显示管理器（ly）

```bash
sudo pacman -S ly
sudo systemctl enable ly
```

### 6. 安装中文字体

```bash
sudo pacman -S noto-fonts-cjk noto-fonts-emoji \
    ttf-sarasa-gothic wqy-microhei \
    adobe-source-han-sans-cn-fonts \
    adobe-source-han-serif-cn-fonts

fc-cache -fv
```

### 7. 安装输入法（fcitx5 + Rime 雾凇拼音）


```bash
sudo pacman -S fcitx5 fcitx5-chinese-addons fcitx5-qt fcitx5-gtk \
    fcitx5-configtool fcitx5-pinyin-zhwiki \
    fcitx5-rime rime-ice
```

环境变量（`~/.config/environment.d/fcitx5.conf`）：
```
XMODIFIERS=@im=fcitx
QT_IM_MODULE=fcitx
SDL_IM_MODULE=fcitx
GLFW_IM_MODULE=ibus
```

自动启动（`~/.config/autostart/fcitx5.desktop`）：
```
[Desktop Entry]
Type=Application
Name=Fcitx5
Exec=fcitx5 -d
```

Rime 配置（`~/.local/share/fcitx5/rime/default.custom.yaml`）：
```yaml
patch:
  schema_list:
    - schema: ice_pinyin
```

### 8. [[Arch Linux 配置]]
---
## 六、注意事项

- EFI 分区挂载时必须挂到 `/mnt/boot`，不能挂到 `/boot`
- `genfstab` 生成后务必检查六条记录是否完整
- HOOKS 中必须包含 `btrfs`
- `GRUB_DISABLE_OS_PROBER=false` 才能探测到 Windows
- swap 分区 UUID 用于休眠，需写入 GRUB 内核参数
- `RDSEED32 is broken` 提示是 Zen5 已知问题，内核自动处理，忽略即可
- 双系统时间同步问题：在 Arch 中执行 `timedatectl set-local-rtc 1`