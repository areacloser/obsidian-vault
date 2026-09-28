
---

## 一、Btrfs 简介

Btrfs（B-Tree File System）是 Linux 上的现代写时复制（CoW，Copy-on-Write）文件系统，由 Oracle 于 2007 年开始开发，目前是 Arch Linux、Fedora 等发行版的主流选择。

**核心优势：**

- 写时复制：修改文件时不覆盖原数据，而是写到新位置，数据更安全
- 内置快照：几乎零成本创建系统快照，可随时回滚
- 透明压缩：读写时自动压缩，对代码/文本文件效果显著
- 子卷管理：灵活划分存储空间，无需提前规划分区大小
- 内置校验：自动检测和修复数据损坏（需RAID配置）
- SSD 优化：原生支持 TRIM/Discard

---

## 二、核心概念

### 写时复制（CoW）

修改文件时，Btrfs 不直接覆盖原有数据块，而是将修改写入新的数据块，再更新元数据指针。好处是崩溃时原数据完好，坏处是频繁小文件写入会产生碎片。

**注意：** 数据库文件（MySQL、SQLite）等频繁随机写入的场景建议对该目录关闭 CoW：

```bash
chattr +C /var/lib/mysql
```

### 子卷（Subvolume）

子卷是 Btrfs 内部的独立文件树，类似轻量级分区，但共享同一个 Btrfs 分区的空间。每个子卷可以独立挂载、独立快照、独立设置挂载选项。

### 快照（Snapshot）

快照是子卷在某一时刻的只读或可写副本，创建时几乎不占空间（CoW 机制保证只记录差异），是 Btrfs 最实用的功能。

---

## 三、挂载选项详解

常用挂载选项说明：

|选项|说明|
|---|---|
|`noatime`|不记录文件访问时间，减少写入，推荐开启|
|`compress=zstd`|使用 zstd 算法透明压缩，推荐|
|`compress=zstd:3`|zstd 压缩级别3，平衡速度和压缩率|
|`ssd`|针对 SSD 优化，检测到 SSD 时自动启用|
|`discard=async`|异步 TRIM，SSD 推荐，比同步 TRIM 性能更好|
|`space_cache=v2`|新版空间缓存，性能更好|
|`subvol=@`|指定挂载的子卷名称|

**推荐的完整挂载选项：**

```
noatime,compress=zstd:3,ssd,discard=async,space_cache=v2,subvol=@
```

---

## 四、常用命令

### 子卷管理

```bash
# 创建子卷
btrfs subvolume create /path/to/subvol

# 列出所有子卷
btrfs subvolume list /

# 删除子卷
btrfs subvolume delete /path/to/subvol

# 查看子卷详情
btrfs subvolume show /path/to/subvol
```

### 快照管理

```bash
# 创建只读快照（推荐用于备份）
btrfs subvolume snapshot -r / /.snapshots/root-$(date +%Y%m%d)

# 创建可写快照（用于回滚操作）
btrfs subvolume snapshot / /.snapshots/root-writable

# 删除快照
btrfs subvolume delete /.snapshots/root-20260614

# 列出所有快照
btrfs subvolume list / | grep snapshots
```

### 文件系统信息

```bash
# 查看使用情况
btrfs filesystem usage /

# 查看文件系统状态
btrfs filesystem show /

# 查看数据块使用情况
btrfs filesystem df /
```

### 维护命令

```bash
# 手动触发碎片整理（慎用，CoW文件系统碎片整理成本高）
btrfs filesystem defragment -r /home

# 数据校验和修复
btrfs scrub start /
btrfs scrub status /

# 均衡数据分布（添加/删除设备后使用）
btrfs balance start /
```

### 压缩相关

```bash
# 对已有文件强制压缩（新挂载compress选项不影响旧文件）
btrfs filesystem defragment -r -czstd /home

# 查看文件压缩率
compsize /home
```

---

## 五、快照回滚方案

### 方案一：手动回滚

系统出问题时，从 Arch ISO 启动：

```bash
# 挂载 Btrfs 顶层
mount /dev/nvme0n1p8 /mnt

# 备份当前损坏的根子卷
mv /mnt/@ /mnt/@broken

# 将快照恢复为新的根子卷
btrfs subvolume snapshot /mnt/@snapshots/root-20260614 /mnt/@

# 重启即可
```

### 方案二：[[snapper 自动快照]]（推荐）

## 六、子卷排除快照的最佳实践

快照时不希望纳入的目录（游戏库、虚拟机镜像、Docker数据），单独建子卷挂载即可自动排除：

```bash
# 在 Btrfs 顶层创建
mount /dev/nvme0n1p8 /mnt/btrfs-top
btrfs subvolume create /mnt/btrfs-top/@steam
btrfs subvolume create /mnt/btrfs-top/@docker
umount /mnt/btrfs-top
```

在 `/etc/fstab` 中添加挂载：

```
UUID=你的UUID  /games  btrfs  noatime,compress=zstd:3,ssd,discard=async,subvol=@steam  0  0
UUID=你的UUID  /var/lib/docker  btrfs  noatime,ssd,discard=async,subvol=@docker  0  0
```

**注意：** Docker 目录建议关闭压缩，镜像层已经压缩过了。

---

## 七、注意事项

- **不要对 `/var/log`、`/var/cache` 做快照**，把它们放在独立子卷里排除在外，否则快照会迅速膨胀
- **数据库目录关闭 CoW**，避免性能下降和碎片问题
- **定期执行 `btrfs scrub`**，建议每月一次，检测和修复潜在数据错误
- **不要把 swap 文件放在 Btrfs 上**（本方案用独立分区做 swap，规避了这个问题）
- **`compress` 选项只对新写入文件生效**，对已有文件需要用 `defragment -czstd` 重新压缩
- **快照不是备份**，快照和源数据在同一磁盘上，磁盘损坏时快照也会丢失，重要数据需要额外备份到其他介质