# Linux Basics

This file contains basic Linux commands that are useful for RHCSA preparation and daily system administration.

## 1. Navigation

Navigation means moving through the Linux filesystem.

```bash
pwd
```

Shows the current directory.

```bash
ls
```

Lists files and directories.

```bash
ls -l
```

Lists files with details such as permissions, owner, group, size and date.

```bash
ls -la
```

Shows all files, including hidden files.

```bash
cd /etc
```

Moves to the `/etc` directory.

```bash
cd ..
```

Moves one level up.

```bash
cd ~
```

Moves to the current user's home directory.

## 2. File and Directory Management

```bash
touch file.txt
```

Creates an empty file.

```bash
mkdir test
```

Creates a directory.

```bash
mkdir -p /backup/daily
```

Creates parent directories if they do not already exist.

```bash
cp file1 file2
```

Copies a file.

```bash
cp -r dir1 dir2
```

Copies a directory recursively.

```bash
mv oldname newname
```

Renames or moves a file/directory.

```bash
rm file.txt
```

Deletes a file.

```bash
rm -r directory
```

Deletes a directory recursively.

## 3. Viewing File Content

```bash
cat file.txt
```

Displays the entire file.

```bash
less file.txt
```

Opens a file page by page.

```bash
head file.txt
```

Shows the first lines of a file.

```bash
tail file.txt
```

Shows the last lines of a file.

```bash
tail -f /var/log/messages
```

Follows a log file in real time.

## 4. Searching Files and Text

```bash
find /etc -name "*.conf"
```

Finds all `.conf` files inside `/etc`.

```bash
find / -type f -user adri
```

Finds all files owned by user `adri`.

```bash
grep "PermitRootLogin" /etc/ssh/sshd_config
```

Searches for text inside a file.

```bash
grep -R "httpd" /etc
```

Searches recursively inside a directory.

## 5. Permissions

Linux permissions control who can read, write or execute files.

```bash
ls -l
```

Example output:

```bash
-rwxr-xr--
```

Meaning:

```text
r = read
w = write
x = execute
```

Permission groups:

```text
user | group | others
```

Example:

```bash
chmod 755 script.sh
```

Sets permissions to:

```text
user: read/write/execute
group: read/execute
others: read/execute
```

```bash
chown adri:adri file.txt
```

Changes the owner and group of a file.

## 6. Processes

```bash
ps aux
```

Shows running processes.

```bash
top
```

Shows live system resource usage.

```bash
kill PID
```

Stops a process by its PID.

```bash
kill -9 PID
```

Forcefully kills a process.

## 7. Disk and Memory

```bash
df -h
```

Shows disk usage in human-readable format.

```bash
du -sh /var/log
```

Shows the size of a directory.

```bash
free -m
```

Shows memory usage in MB.

## 8. System Information

```bash
hostnamectl
```

Shows hostname and system information.

```bash
uname -r
```

Shows the kernel version.

```bash
uptime
```

Shows how long the system has been running.

## 9. Useful RHCSA Notes

Important paths:

```text
/etc        configuration files
/var/log    log files
/home       user home directories
/root       root user's home directory
/tmp        temporary files
/usr/bin    common user commands
/usr/sbin   system administration commands
```

Common troubleshooting commands:

```bash
systemctl status service_name
journalctl -xe
journalctl -u service_name
ss -tulpn
df -h
free -m
```

## Practice Tasks

1. Create a directory called `/practice`.
2. Create a file called `notes.txt` inside `/practice`.
3. Copy `/etc/hosts` to `/practice`.
4. Search for the word `localhost` inside `/practice/hosts`.
5. Check the permissions of all files inside `/practice`.
6. Delete the directory `/practice`.
