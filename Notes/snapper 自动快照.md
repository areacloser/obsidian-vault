**安装：**

```bash
sudo pacman -S snapper snap-pac
```

**创建配置：**

```bash
# 为根目录创建 snapper 配置
sudo snapper -c root create-config /

# 为 home 创建配置
sudo snapper -c home create-config /home
```

**snap-pac** 会在每次 pacman 操作前后自动创建快照，无需手动干预。

**常用 snapper 命令：**

```bash
# 列出所有快照
snapper -c root list

# 手动创建快照
snapper -c root create --description "安装桌面环境前"

# 比较两个快照的差异
snapper -c root diff 1..2

# 回滚到指定快照（需重启生效）
snapper -c root undochange 1..0

# 删除快照
snapper -c root delete 3
```

**配置自动清理策略**（`/etc/snapper/configs/root`）：

```
TIMELINE_CREATE="yes"
TIMELINE_CLEANUP="yes"
NUMBER_LIMIT="5-10"
TIMELINE_LIMIT_HOURLY="3"
TIMELINE_LIMIT_DAILY="7"
TIMELINE_LIMIT_WEEKLY="2"
TIMELINE_LIMIT_MONTHLY="1"
```

**启用定时快照：**

```bash
sudo systemctl enable --now snapper-timeline.timer
sudo systemctl enable --now snapper-cleanup.timer
```

---