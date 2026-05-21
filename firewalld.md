# Firewalld

This file contains firewalld notes used in RHCSA and Linux administration.

## 1. What is Firewalld?

Firewalld is the firewall management service used in RHEL, AlmaLinux and Rocky Linux.

It controls:
- allowed ports
- allowed services
- network zones
- traffic filtering

Main service:

```bash
firewalld
```

Check status:

```bash
systemctl status firewalld
```

## 2. Start and Enable Firewalld

Start service:

```bash
systemctl start firewalld
```

Enable at boot:

```bash
systemctl enable firewalld
```

Verify:

```bash
systemctl status firewalld
```

## 3. Firewall Zones

Zones define trust levels.

View active zones:

```bash
firewall-cmd --get-active-zones
```

View default zone:

```bash
firewall-cmd --get-default-zone
```

Common zones:

```text
public
home
internal
trusted
dmz
block
drop
```

View zone configuration:

```bash
firewall-cmd --list-all
```

## 4. Runtime vs Permanent Rules

Firewalld has two configurations:

```text
runtime    = temporary
permanent  = survives reboot
```

Permanent changes require reload.

Example:

```bash
firewall-cmd --add-service=http --permanent
firewall-cmd --reload
```

## 5. Allow Services

Allow HTTP:

```bash
firewall-cmd --add-service=http --permanent
```

Allow HTTPS:

```bash
firewall-cmd --add-service=https --permanent
```

Allow SSH:

```bash
firewall-cmd --add-service=ssh --permanent
```

Reload firewall:

```bash
firewall-cmd --reload
```

Verify:

```bash
firewall-cmd --list-all
```

## 6. Allow Ports

Open custom TCP port:

```bash
firewall-cmd --add-port=8080/tcp --permanent
```

Open UDP port:

```bash
firewall-cmd --add-port=53/udp --permanent
```

Reload:

```bash
firewall-cmd --reload
```

## 7. Remove Rules

Remove service:

```bash
firewall-cmd --remove-service=http --permanent
```

Remove port:

```bash
firewall-cmd --remove-port=8080/tcp --permanent
```

Reload firewall:

```bash
firewall-cmd --reload
```

## 8. List Rules

List all rules:

```bash
firewall-cmd --list-all
```

List services:

```bash
firewall-cmd --list-services
```

List ports:

```bash
firewall-cmd --list-ports
```

## 9. Network Interfaces and Zones

Assign interface to zone:

```bash
firewall-cmd --zone=internal --change-interface=eth0 --permanent
```

Reload:

```bash
firewall-cmd --reload
```

Verify:

```bash
firewall-cmd --get-active-zones
```

## 10. Rich Rules

Example:

```bash
firewall-cmd --add-rich-rule='rule family="ipv4" source address="192.168.1.10" accept'
```

Explanation:

```text
Allows traffic from a specific IP.
```

## 11. Masquerading (NAT)

Enable masquerade:

```bash
firewall-cmd --add-masquerade --permanent
```

Reload:

```bash
firewall-cmd --reload
```

Verify:

```bash
firewall-cmd --list-all
```

## 12. Useful Files

```text
/etc/firewalld/
/usr/lib/firewalld/
```

## 13. Troubleshooting

Check if firewalld is running:

```bash
systemctl status firewalld
```

Check open ports:

```bash
ss -tulpn
```

Check firewall rules:

```bash
firewall-cmd --list-all
```

Test connectivity:

```bash
ping
curl
telnet
nc
```

## 14. Example Troubleshooting Workflow

Problem:

```text
Website does not open.
```

Check service:

```bash
systemctl status httpd
```

Check listening ports:

```bash
ss -tulpn | grep :80
```

Check firewall:

```bash
firewall-cmd --list-all
```

Open HTTP:

```bash
firewall-cmd --add-service=http --permanent
```

Reload firewall:

```bash
firewall-cmd --reload
```

## 15. Important RHCSA Commands

```bash
firewall-cmd --list-all
firewall-cmd --reload
firewall-cmd --add-service
firewall-cmd --add-port
firewall-cmd --remove-service
firewall-cmd --get-active-zones
```

## 16. Common Ports

```text
22   SSH
80   HTTP
443  HTTPS
53   DNS
25   SMTP
3306 MySQL/MariaDB
```

## Practice Tasks

1. Start and enable firewalld.
2. Allow HTTP and HTTPS.
3. Open port 8080/tcp.
4. Remove port 8080/tcp.
5. Verify firewall rules.
6. Assign interface to internal zone.
7. Configure masquerading.
8. Verify listening ports.
