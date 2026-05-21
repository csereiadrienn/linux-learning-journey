# Linux Boot Process

This file contains notes about the Linux boot process and boot troubleshooting.

## 1. Linux Boot Process Overview

Boot flow:

```text
BIOS/UEFI
    ↓
Bootloader (GRUB2)
    ↓
Kernel
    ↓
initramfs
    ↓
systemd (PID 1)
    ↓
Services and Targets
    ↓
User Login
```

## 2. BIOS vs UEFI

### BIOS

Older firmware interface.

Characteristics:
- MBR partitions
- older systems

### UEFI

Modern firmware interface.

Characteristics:
- GPT partitions
- secure boot support
- faster boot process

Check firmware type:

```bash
ls /sys/firmware/efi
```

If directory exists:

```text
System uses UEFI.
```

## 3. GRUB2

GRUB2 is the Linux bootloader.

Responsibilities:
- loads kernel
- loads initramfs
- provides boot menu

Main configuration:

```text
/etc/default/grub
```

GRUB files:

```text
/boot/grub2/
/boot/grub2/grub.cfg
```

## 4. Kernel

The kernel is the core of Linux.

Check kernel version:

```bash
uname -r
```

Installed kernels:

```bash
rpm -q kernel
```

## 5. initramfs

Temporary filesystem loaded during boot.

Purpose:
- load drivers
- mount real root filesystem

Files:

```text
/boot/initramfs-*
```

## 6. systemd

systemd is the first userspace process.

PID:

```bash
PID 1
```

Verify:

```bash
ps -p 1
```

## 7. Boot Targets

Check current target:

```bash
systemctl get-default
```

Common targets:

```text
multi-user.target
graphical.target
rescue.target
emergency.target
```

Switch target temporarily:

```bash
systemctl isolate multi-user.target
```

Set default target:

```bash
systemctl set-default multi-user.target
```

## 8. Rescue and Emergency Modes

### Rescue Mode

Single-user mode with basic services.

Boot target:

```text
rescue.target
```

### Emergency Mode

Minimal environment.

Only root filesystem available.

Used for:
- broken fstab
- filesystem repair
- severe boot issues

## 9. Boot Logs

Current boot logs:

```bash
journalctl -b
```

Previous boot:

```bash
journalctl -b -1
```

Kernel messages:

```bash
dmesg
```

## 10. Troubleshooting fstab

Main file:

```text
/etc/fstab
```

Broken fstab may cause boot failure.

Verify safely:

```bash
mount -a
```

If no errors:
- fstab is probably valid

## 11. Filesystem Repair

Repair XFS:

```bash
xfs_repair /dev/sdb1
```

Repair EXT4:

```bash
fsck /dev/sdb1
```

## 12. Change Root Password from GRUB

At GRUB menu:
- press `e`
- modify kernel line

Add:

```text
rd.break
```

Boot:
- CTRL + X

Remount filesystem:

```bash
mount -o remount,rw /sysroot
```

Chroot:

```bash
chroot /sysroot
```

Change password:

```bash
passwd root
```

Create SELinux relabel:

```bash
touch /.autorelabel
```

Exit and reboot:

```bash
exit
reboot
```

## 13. Important RHCSA Commands

```bash
uname -r
journalctl -b
dmesg
systemctl get-default
mount -a
fsck
xfs_repair
```

## 14. Common Boot Problems

### System enters emergency mode

Possible causes:
- broken fstab
- missing UUID
- corrupted filesystem

### Kernel panic

Possible causes:
- corrupted kernel
- hardware issues
- filesystem corruption

### Service fails at boot

Possible causes:
- wrong configuration
- missing dependency
- SELinux issue

## 15. Example Troubleshooting Workflow

Problem:

```text
System boots into emergency mode.
```

Check:

```bash
journalctl -xb
```

Check fstab:

```bash
cat /etc/fstab
```

Verify disks:

```bash
lsblk
```

Check UUIDs:

```bash
blkid
```

Repair filesystem if needed:

```bash
fsck /dev/sdb1
```

## 16. Practice Tasks

1. Check current kernel version.
2. Verify default target.
3. Switch to multi-user target.
4. View previous boot logs.
5. Break and repair fstab.
6. Enter rescue mode.
7. Repair filesystem.
8. Reset root password from GRUB.
