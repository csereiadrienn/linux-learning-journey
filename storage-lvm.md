# Storage and LVM

This file contains notes about storage management and Logical Volume Manager (LVM) used in RHCSA.

## 1. Check Disks and Partitions

```bash
lsblk
```

Shows disks, partitions and mount points.

Example:

```text
sda
├─sda1 /boot
├─sda2 /
sdb
```

```bash
fdisk -l
```

Shows detailed partition information.

```bash
blkid
```

Shows UUIDs and filesystem types.

## 2. Create Partitions

```bash
fdisk /dev/sdb
```

Used to create or manage partitions.

Common commands inside fdisk:

```text
n = new partition
d = delete partition
p = print table
w = write changes
q = quit
```

After creating partitions:

```bash
partprobe
```

Reloads the partition table without rebooting.

## 3. Filesystems

Create XFS filesystem:

```bash
mkfs.xfs /dev/sdb1
```

Create EXT4 filesystem:

```bash
mkfs.ext4 /dev/sdb1
```

## 4. Mount Filesystems

Create mount point:

```bash
mkdir /data
```

Mount filesystem:

```bash
mount /dev/sdb1 /data
```

Verify mount:

```bash
df -h
```

## 5. Persistent Mounts

Linux uses `/etc/fstab` for automatic mounting at boot.

Open file:

```bash
vi /etc/fstab
```

Example:

```text
UUID=xxxxx /data xfs defaults 0 0
```

Test fstab safely:

```bash
mount -a
```

If there are no errors, configuration is correct.

## 6. LVM Concepts

LVM allows flexible storage management.

Main components:

```text
PV = Physical Volume
VG = Volume Group
LV = Logical Volume
```

Flow:

```text
Disk -> PV -> VG -> LV -> Filesystem -> Mount
```

## 7. Create Physical Volume (PV)

```bash
pvcreate /dev/sdb
```

Verify:

```bash
pvs
```

## 8. Create Volume Group (VG)

```bash
vgcreate vgdata /dev/sdb
```

Verify:

```bash
vgs
```

Example with PE size:

```bash
vgcreate -s 8M vgdata /dev/sdb
```

Explanation:

```text
-s 8M sets Physical Extent size to 8 MB.
```

## 9. Create Logical Volume (LV)

Create LV by size:

```bash
lvcreate -L 2G -n lvdata vgdata
```

Explanation:

```text
-L = size
-n = logical volume name
```

Create LV using extents:

```bash
lvcreate -l 50 -n lvshare vgdata
```

Verify:

```bash
lvs
```

## 10. Create Filesystem on LV

```bash
mkfs.xfs /dev/vgdata/lvdata
```

## 11. Mount LVM

```bash
mkdir /storage
mount /dev/vgdata/lvdata /storage
```

Persistent mount:

```bash
blkid
```

Add to `/etc/fstab`:

```text
UUID=xxxxx /storage xfs defaults 0 0
```

Then:

```bash
mount -a
```

## 12. Extend Logical Volume

Extend LV:

```bash
lvextend -L +1G /dev/vgdata/lvdata
```

Grow XFS filesystem:

```bash
xfs_growfs /storage
```

For EXT4:

```bash
resize2fs /dev/vgdata/lvdata
```

## 13. Swap

Check swap:

```bash
swapon --show
```

Create swap file:

```bash
fallocate -l 1G /swapfile
```

Permissions:

```bash
chmod 600 /swapfile
```

Create swap:

```bash
mkswap /swapfile
```

Enable swap:

```bash
swapon /swapfile
```

Persistent swap:

Add to `/etc/fstab`

```text
/swapfile swap swap defaults 0 0
```

## 14. Important RHCSA Commands

```bash
lsblk
blkid
df -h
mount
umount
pvs
vgs
lvs
mount -a
```

## 15. Troubleshooting

Check mounted filesystems:

```bash
mount | grep data
```

Check UUID:

```bash
blkid
```

Check filesystem:

```bash
xfs_repair /dev/sdb1
```

Check logs:

```bash
journalctl -xe
```

## Practice Tasks

1. Create a new partition on `/dev/sdb`.
2. Format it with XFS.
3. Mount it permanently on `/data`.
4. Create a Volume Group called `vgdata`.
5. Create a Logical Volume called `lvbackup`.
6. Mount it on `/backup`.
7. Extend the LV by 1 GB.
8. Verify all mounts.
