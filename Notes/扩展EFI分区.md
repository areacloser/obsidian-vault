当 Windows 自带的 EFI 分区空间不足（通常默认仅 100MB）时，需要扩展以同时容纳 Windows Boot Manager 和 GRUB。

---

## 一、准备工具

### 软件

- **微PE**：Windows PE 维护系统
  - [微PE下载地址](https://www.wepe.com.cn/download.html)
- **Ventoy**：多 ISO 启动盘工具
  - [Ventoy下载地址](https://www.ventoy.net/en/download.html)

### 硬件

- 一个空 U 盘（建议 ≥ 8GB）

---

## 二、制作启动盘

1. 打开 Ventoy，选择 U 盘，点击”安装”（会清空 U 盘）
2. 将微PE ISO 和 Arch Linux ISO 复制到 U 盘的 Ventoy 分区
3. 完成后 U 盘可同时启动微PE 和 Arch ISO

---

## 三、进入 PE 扩展分区

1. 从 U 盘启动，选择微PE
2. 打开”分区工具（DiskGenius）”
3. 选中 ESP（EFI 分区，通常 100MB FAT32）
4. 右键 → “调整分区大小”
5. 从 C 盘或其他分区划出空闲空间（建议扩展到 ≥ 4GB）
6. 点击”开始”执行，等待完成
7. 重启，确认 Windows 仍能正常启动

---

## 四、为什么要扩展 EFI 分区

- Windows 默认 ESP 通常只有 100MB，仅够放 Windows Boot Manager
- 双系统需要额外存放 GRUB 内核镜像（linux-zen 约 120MB+）
- 4GB 是 Arch Wiki 推荐值，足够存放多个内核和 fallback initramfs

---

## 五、相关笔记

- [[Arch Linux & Windows 双系统安装]] — 主安装流程
- [[Btrfs文件系统]] — Linux 分区方案