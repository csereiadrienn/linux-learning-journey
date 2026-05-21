# SSH and Remote Access

This file contains SSH and remote access notes useful for RHCSA and Linux administration.

## 1. What is SSH?

SSH stands for:

```text
Secure Shell
```

Used for:
- remote server administration
- secure command execution
- file transfers
- tunneling

Default SSH port:

```text
TCP 22
```

Main service:

```bash
sshd
```

## 2. Check SSH Service

Check service status:

```bash
systemctl status sshd
```

Start service:

```bash
systemctl start sshd
```

Enable at boot:

```bash
systemctl enable sshd
```

## 3. Connect to Remote Server

Basic syntax:

```bash
ssh user@192.168.1.10
```

Example:

```bash
ssh root@192.168.1.20
```

Connect using hostname:

```bash
ssh user@server.example.com
```

## 4. SSH Configuration File

Main config file:

```text
/etc/ssh/sshd_config
```

View config:

```bash
vi /etc/ssh/sshd_config
```

After changes:

```bash
systemctl restart sshd
```

## 5. Important SSH Settings

Change SSH port:

```text
Port 2222
```

Disable root login:

```text
PermitRootLogin no
```

Disable password authentication:

```text
PasswordAuthentication no
```

Allow only specific users:

```text
AllowUsers adri admin
```

## 6. SSH Keys

Generate key pair:

```bash
ssh-keygen
```

Default location:

```text
~/.ssh/
```

Files:

```text
id_rsa       = private key
id_rsa.pub   = public key
```

## 7. Copy SSH Key

Copy public key to server:

```bash
ssh-copy-id user@192.168.1.10
```

Manual method:

```bash
cat ~/.ssh/id_rsa.pub
```

Paste into:

```text
~/.ssh/authorized_keys
```

## 8. SSH Permissions

Correct permissions are very important.

Directory:

```bash
chmod 700 ~/.ssh
```

Authorized keys:

```bash
chmod 600 ~/.ssh/authorized_keys
```

## 9. SCP

Copy file to remote server:

```bash
scp file.txt user@192.168.1.10:/backup
```

Copy directory:

```bash
scp -r /data user@192.168.1.10:/backup
```

## 10. SFTP

Connect with SFTP:

```bash
sftp user@192.168.1.10
```

Useful commands:

```text
put
get
ls
cd
```

## 11. SSH Port Troubleshooting

Check listening ports:

```bash
ss -tulpn | grep ssh
```

or:

```bash
ss -tulpn | grep :22
```

## 12. Firewall and SSH

Allow SSH:

```bash
firewall-cmd --add-service=ssh --permanent
```

Reload firewall:

```bash
firewall-cmd --reload
```

## 13. SELinux and SSH

Check SSH SELinux ports:

```bash
semanage port -l | grep ssh
```

Add custom SSH port:

```bash
semanage port -a -t ssh_port_t -p tcp 2222
```

## 14. Restart SSH on Custom Port

Example workflow:

1. Change SSH port in:

```text
/etc/ssh/sshd_config
```

2. Allow port in firewall:

```bash
firewall-cmd --add-port=2222/tcp --permanent
```

3. Configure SELinux:

```bash
semanage port -a -t ssh_port_t -p tcp 2222
```

4. Reload firewall:

```bash
firewall-cmd --reload
```

5. Restart SSH:

```bash
systemctl restart sshd
```

## 15. SSH Troubleshooting

Check service:

```bash
systemctl status sshd
```

Check logs:

```bash
journalctl -u sshd
```

Check ports:

```bash
ss -tulpn | grep ssh
```

Check firewall:

```bash
firewall-cmd --list-all
```

Check SELinux:

```bash
ausearch -m AVC -ts recent
```

## 16. Example Troubleshooting

Problem:

```text
Cannot connect to SSH after changing port.
```

Check SSH config:

```bash
sshd -t
```

Check listening ports:

```bash
ss -tulpn | grep ssh
```

Check firewall:

```bash
firewall-cmd --list-all
```

Check SELinux:

```bash
semanage port -l | grep ssh
```

## 17. Important RHCSA Commands

```bash
ssh
scp
sftp
ssh-keygen
ssh-copy-id
systemctl status sshd
ss -tulpn
```

## 18. Practice Tasks

1. Start and enable SSH service.
2. Connect to remote server.
3. Generate SSH keys.
4. Configure passwordless authentication.
5. Change SSH port to 2222.
6. Configure firewall for new port.
7. Configure SELinux for new SSH port.
8. Troubleshoot SSH connectivity.
