# Systemd and Services

This file contains notes about systemd, services and logs used in RHCSA.

## 1. What is systemd?

Systemd is the service manager used by modern Linux distributions.

It manages:
- services
- boot process
- targets
- logs
- timers
- background processes

Main process:

```bash
PID 1
```

Check:

```bash
ps -p 1
```

## 2. Service Management

Check service status:

```bash
systemctl status httpd
```

Start a service:

```bash
systemctl start httpd
```

Stop a service:

```bash
systemctl stop httpd
```

Restart a service:

```bash
systemctl restart httpd
```

Reload configuration without full restart:

```bash
systemctl reload httpd
```

Enable service at boot:

```bash
systemctl enable httpd
```

Disable service at boot:

```bash
systemctl disable httpd
```

Check if service is enabled:

```bash
systemctl is-enabled httpd
```

Check if service is active:

```bash
systemctl is-active httpd
```

## 3. List Services

List running services:

```bash
systemctl list-units --type=service
```

List failed services:

```bash
systemctl --failed
```

## 4. Boot Targets

Targets are groups of services.

Check current target:

```bash
systemctl get-default
```

Common targets:

```text
multi-user.target = command line mode
graphical.target = GUI mode
rescue.target = rescue mode
emergency.target = emergency shell
```

Switch target temporarily:

```bash
systemctl isolate multi-user.target
```

Set default target permanently:

```bash
systemctl set-default multi-user.target
```

## 5. Logs with journalctl

Systemd uses journald for logs.

Show all logs:

```bash
journalctl
```

Show recent logs:

```bash
journalctl -xe
```

Show logs for specific service:

```bash
journalctl -u httpd
```

Follow logs live:

```bash
journalctl -f
```

Show logs since boot:

```bash
journalctl -b
```

Show previous boot logs:

```bash
journalctl -b -1
```

## 6. Manage Hostname

Check hostname:

```bash
hostnamectl
```

Set hostname:

```bash
hostnamectl set-hostname server1.example.com
```

Verify:

```bash
hostname
```

## 7. Timedatectl

Check date and timezone:

```bash
timedatectl
```

Set timezone:

```bash
timedatectl set-timezone Europe/Bucharest
```

Enable NTP sync:

```bash
timedatectl set-ntp true
```

## 8. Service Files

Service files are stored in:

```text
/usr/lib/systemd/system/
/etc/systemd/system/
```

View service file:

```bash
systemctl cat httpd
```

## 9. Reload systemd

After modifying service files:

```bash
systemctl daemon-reload
```

Explanation:

```text
Reloads systemd configuration files.
```

## 10. Mask Services

Masking prevents services from starting.

Mask service:

```bash
systemctl mask httpd
```

Unmask service:

```bash
systemctl unmask httpd
```

## 11. Important RHCSA Commands

```bash
systemctl status
systemctl restart
systemctl enable
systemctl disable
journalctl
hostnamectl
timedatectl
```

## 12. Troubleshooting

Check failed services:

```bash
systemctl --failed
```

Check logs:

```bash
journalctl -xe
```

Check specific service:

```bash
systemctl status sshd
```

Verify listening ports:

```bash
ss -tulpn
```

## 13. Example Troubleshooting Workflow

Problem:
Apache website does not work.

Steps:

```bash
systemctl status httpd
```

```bash
journalctl -u httpd
```

```bash
ss -tulpn | grep :80
```

```bash
firewall-cmd --list-all
```

```bash
ausearch -m AVC -ts recent
```

## Practice Tasks

1. Start and enable the `httpd` service.
2. Check if the service is active.
3. Stop the service.
4. View logs for `httpd`.
5. Change the hostname.
6. Set the default target to `multi-user.target`.
7. List failed services.
8. Check previous boot logs.
