# Users, Groups and Permissions

This file contains notes about Linux users, groups and permissions for RHCSA.

## 1. Linux Users

Linux is a multi-user operating system.

Each user has:
- username
- UID
- home directory
- shell

Important files:

```text
/etc/passwd
/etc/shadow
/etc/group
```

## 2. View Users

Show current user:

```bash
whoami
```

Show logged in users:

```bash
who
```

View passwd file:

```bash
cat /etc/passwd
```

Example:

```text
adri:x:1001:1001:Adri:/home/adri:/bin/bash
```

Structure:

```text
username:password_placeholder:UID:GID:comment:home:shell
```

## 3. Create Users

Create user:

```bash
useradd user1
```

Create user with home directory:

```bash
useradd -m user1
```

Set password:

```bash
passwd user1
```

Create user with custom shell:

```bash
useradd -s /bin/bash user1
```

Create user with UID:

```bash
useradd -u 2000 user1
```

## 4. Delete Users

Delete user:

```bash
userdel user1
```

Delete user and home directory:

```bash
userdel -r user1
```

## 5. Groups

View groups:

```bash
cat /etc/group
```

Create group:

```bash
groupadd developers
```

Add user to group:

```bash
usermod -aG developers user1
```

Explanation:

```text
-a = append
-G = secondary groups
```

View user groups:

```bash
id user1
```

or:

```bash
groups user1
```

## 6. Password Aging

View password settings:

```bash
chage -l user1
```

Set password expiration:

```bash
chage -M 30 user1
```

Explanation:

```text
Password expires after 30 days.
```

## 7. Switch Users

Switch user:

```bash
su - user1
```

Run command as root:

```bash
sudo command
```

## 8. Sudo Access

Edit sudoers safely:

```bash
visudo
```

Example:

```text
user1 ALL=(ALL) ALL
```

Group example:

```text
%developers ALL=(ALL) ALL
```

## 9. Linux Permissions

View permissions:

```bash
ls -l
```

Example:

```text
-rwxr-x---
```

Structure:

```text
user | group | others
```

Permission meanings:

```text
r = read
w = write
x = execute
```

## 10. Numeric Permissions

Examples:

```text
7 = rwx
6 = rw-
5 = r-x
4 = r--
```

Example:

```bash
chmod 755 script.sh
```

Meaning:

```text
Owner = rwx
Group = r-x
Others = r-x
```

Another example:

```bash
chmod 640 file.txt
```

## 11. Ownership

Change owner:

```bash
chown user1 file.txt
```

Change owner and group:

```bash
chown user1:developers file.txt
```

Change group:

```bash
chgrp developers file.txt
```

## 12. Special Permissions

### SUID

```bash
chmod u+s file
```

Example:

```text
-rwsr-xr-x
```

Program runs with owner's privileges.

### SGID

```bash
chmod g+s directory
```

Files inherit group ownership.

### Sticky Bit

```bash
chmod +t /shared
```

Example:

```text
drwxrwxrwt
```

Users can only delete their own files.

## 13. Umask

Check umask:

```bash
umask
```

Example:

```text
0022
```

Meaning:

```text
Default permissions are restricted.
```

## 14. ACLs

ACLs allow advanced permissions.

View ACL:

```bash
getfacl file.txt
```

Set ACL:

```bash
setfacl -m u:user1:rwx file.txt
```

Remove ACL:

```bash
setfacl -x u:user1 file.txt
```

## 15. Important RHCSA Commands

```bash
useradd
passwd
usermod
groupadd
id
chmod
chown
chgrp
visudo
setfacl
getfacl
```

## 16. Troubleshooting

Check user info:

```bash
id user1
```

Check permissions:

```bash
ls -l
```

Check ACLs:

```bash
getfacl file.txt
```

Check sudo access:

```bash
sudo -l
```

## 17. Example Troubleshooting

Problem:

```text
User cannot edit file.
```

Check ownership:

```bash
ls -l file.txt
```

Check ACL:

```bash
getfacl file.txt
```

Fix permissions:

```bash
chmod 664 file.txt
```

Or:

```bash
setfacl -m u:user1:rw file.txt
```

## 18. Practice Tasks

1. Create user `analyst`.
2. Create group `admins`.
3. Add user to group.
4. Configure password expiration.
5. Create shared directory.
6. Configure SGID on directory.
7. Configure ACL for another user.
8. Configure sudo access.
9. Verify permissions.
