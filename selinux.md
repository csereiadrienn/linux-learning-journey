# SELinux

This file contains SELinux notes for RHCSA and Linux administration.

## 1. What is SELinux?

SELinux stands for:

```text
Security-Enhanced Linux
```

It adds an additional security layer on top of standard Linux permissions.

Even if a user has normal Linux permissions, SELinux can still deny access.

SELinux controls:
- files
- ports
- services
- processes
- network access

## 2. SELinux Modes

Check current mode:

```bash
getenforce
```

Detailed status:

```bash
sestatus
```

Possible modes:

```text
Enforcing  = SELinux active and blocking
Permissive = logs actions but does not block
Disabled   = SELinux disabled
```

## 3. Change SELinux Mode

Temporary change:

```bash
setenforce 0
```

Sets SELinux to permissive.

```bash
setenforce 1
```

Sets SELinux to enforcing.

Verify:

```bash
getenforce
```

## 4. Permanent Configuration

SELinux configuration file:

```bash
/etc/selinux/config
```

Example:

```text
SELINUX=enforcing
```

Possible values:

```text
enforcing
permissive
disabled
```

## 5. Contexts

SELinux uses contexts.

View contexts:

```bash
ls -Z
```

Example:

```text
system_u:object_r:httpd_sys_content_t:s0
```

Structure:

```text
user:role:type:level
```

Most important field:

```text
TYPE
```

Example:

```text
httpd_sys_content_t
```

This allows Apache to read files.

## 6. Change Contexts

Temporary change:

```bash
chcon -t httpd_sys_content_t file.html
```

Problem:

```text
Changes are NOT persistent.
```

After restorecon or relabel, changes disappear.

## 7. Persistent Contexts

Use semanage.

Example:

```bash
semanage fcontext -a -t httpd_sys_content_t "/web(/.*)?"
```

Apply context:

```bash
restorecon -Rv /web
```

Verify:

```bash
ls -Z /web
```

## 8. Writable Contexts

Allow Apache write access:

```bash
semanage fcontext -a -t httpd_sys_rw_content_t "/uploads(/.*)?"
```

Apply:

```bash
restorecon -Rv /uploads
```

## 9. SELinux Ports

View allowed ports:

```bash
semanage port -l
```

Check HTTP ports:

```bash
semanage port -l | grep http
```

Add new HTTP port:

```bash
semanage port -a -t http_port_t -p tcp 8080
```

Modify port:

```bash
semanage port -m -t http_port_t -p tcp 8080
```

Delete port:

```bash
semanage port -d -t http_port_t -p tcp 8080
```

## 10. SELinux Booleans

Booleans enable or disable SELinux behaviors.

View booleans:

```bash
getsebool -a
```

Search HTTP booleans:

```bash
getsebool -a | grep http
```

Example:

```bash
setsebool -P httpd_can_network_connect on
```

Explanation:

```text
Allows Apache to connect to network services.
```

Important:

```text
-P = persistent
```

Another example:

```bash
setsebool -P httpd_enable_homedirs on
```

## 11. Troubleshooting SELinux

View SELinux denials:

```bash
ausearch -m AVC -ts recent
```

View logs:

```bash
journalctl -xe
```

Common workflow:

1. Service does not work
2. Check service status
3. Check firewall
4. Check SELinux logs
5. Check contexts
6. Check ports
7. Check booleans

## 12. Example Troubleshooting

Problem:

```text
Apache cannot access /web/index.html
```

Check context:

```bash
ls -Z /web
```

Fix:

```bash
semanage fcontext -a -t httpd_sys_content_t "/web(/.*)?"
```

Apply:

```bash
restorecon -Rv /web
```

## 13. Important RHCSA Commands

```bash
getenforce
sestatus
ls -Z
chcon
restorecon
semanage
ausearch
getsebool
setsebool
```

## 14. Useful SELinux Types

```text
httpd_sys_content_t
httpd_sys_rw_content_t
ssh_port_t
http_port_t
```

## 15. Common Mistakes

Using:

```bash
chcon
```

instead of:

```bash
semanage fcontext
```

Reason:

```text
chcon is not persistent.
```

## Practice Tasks

1. Check SELinux mode.
2. Create `/web`.
3. Configure SELinux context for Apache.
4. Allow Apache to use port 8080.
5. Configure writable uploads directory.
6. Enable Apache network connections.
7. Verify SELinux logs.
8. Restore contexts.
