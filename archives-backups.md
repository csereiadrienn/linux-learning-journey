# Archives, Compression and Backups

This file contains notes about archiving, compression and backup tools in Linux.

## 1. What is Archiving?

Archiving combines multiple files into a single file.

Common tool:

```bash
tar
```

Important:

```text
tar does NOT compress by default
```

Compression can be added separately.

## 2. Create Archives

Create archive:

```bash
tar -cvf archive.tar /etc
```

Explanation:

```text
-c = create
-v = verbose
-f = filename
```

Example:

```bash
tar -cvf backup.tar /home
```

## 3. Extract Archives

Extract archive:

```bash
tar -xvf backup.tar
```

Explanation:

```text
-x = extract
```

Extract to specific directory:

```bash
tar -xvf backup.tar -C /restore
```

## 4. List Archive Contents

```bash
tar -tvf backup.tar
```

## 5. Gzip Compression

Create compressed archive:

```bash
tar -zcvf backup.tar.gz /home
```

Explanation:

```text
-z = gzip compression
```

Extract gzip archive:

```bash
tar -zxvf backup.tar.gz
```

## 6. Bzip2 Compression

Create bzip2 archive:

```bash
tar -jcvf backup.tar.bz2 /home
```

Extract:

```bash
tar -jxvf backup.tar.bz2
```

## 7. XZ Compression

Create xz archive:

```bash
tar -Jcvf backup.tar.xz /home
```

Extract:

```bash
tar -Jxvf backup.tar.xz
```

## 8. Compression Tools

Compress file with gzip:

```bash
gzip file.txt
```

Decompress:

```bash
gunzip file.txt.gz
```

Compress with bzip2:

```bash
bzip2 file.txt
```

Decompress:

```bash
bunzip2 file.txt.bz2
```

Compress with xz:

```bash
xz file.txt
```

Decompress:

```bash
unxz file.txt.xz
```

## 9. Copy Files with rsync

Basic syntax:

```bash
rsync -av source/ destination/
```

Explanation:

```text
-a = archive mode
-v = verbose
```

Example:

```bash
rsync -av /home/ /backup/home/
```

Remote rsync:

```bash
rsync -av /data user@192.168.1.10:/backup
```

## 10. SCP

Copy file over SSH:

```bash
scp file.txt user@192.168.1.10:/backup
```

Copy directory:

```bash
scp -r /data user@192.168.1.10:/backup
```

## 11. Restic Backups

Restic is a modern backup solution.

Install:

```bash
dnf install restic -y
```

Initialize repository:

```bash
restic -r /backup/restic init
```

Backup data:

```bash
restic -r /backup/restic backup /etc /home
```

View snapshots:

```bash
restic -r /backup/restic snapshots
```

Restore backup:

```bash
restic -r /backup/restic restore latest --target /restore
```

## 12. Restic over SSH

Initialize remote repository:

```bash
restic -r sftp:root@192.168.1.20:/backup/restic init
```

Backup remotely:

```bash
restic -r sftp:root@192.168.1.20:/backup/restic backup /etc
```

## 13. Cron Backups

Example cron job:

```bash
crontab -e
```

Example:

```text
0 3 * * * /root/backup.sh
```

Meaning:

```text
Runs every day at 03:00.
```

## 14. Important Backup Paths

```text
/etc
/home
/var/www
/var/lib
```

## 15. Important RHCSA Commands

```bash
tar
gzip
gunzip
rsync
scp
restic
```

## 16. Troubleshooting

Check archive:

```bash
tar -tvf backup.tar
```

Check backup logs:

```bash
journalctl -xe
```

Check cron:

```bash
systemctl status crond
```

Verify backup files:

```bash
ls -lh /backup
```

## 17. Example Troubleshooting

Problem:

```text
Cron backup does not run.
```

Check cron service:

```bash
systemctl status crond
```

Check cron entries:

```bash
crontab -l
```

Check script permissions:

```bash
ls -l /root/backup.sh
```

Make executable:

```bash
chmod +x /root/backup.sh
```

## 18. Practice Tasks

1. Create archive of `/etc`.
2. Compress archive with gzip.
3. Extract archive to `/restore`.
4. Copy files using rsync.
5. Configure remote SCP copy.
6. Install Restic.
7. Create Restic repository.
8. Backup `/home`.
9. Restore latest snapshot.
10. Create daily cron backup.
