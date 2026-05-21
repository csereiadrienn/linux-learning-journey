# Networking Fundamentals

This file contains basic Linux networking notes useful for RHCSA and system administration.

## 1. Check Network Interfaces

Show interfaces:

```bash
ip a
```

or:

```bash
ip addr
```

Example:

```text
eth0
ens33
enp0s3
```

These are network interface names.

## 2. Check IP Address

```bash
ip a
```

Shows:
- IPv4
- IPv6
- interface status

Example:

```text
inet 192.168.1.100/24
```

Explanation:

```text
/24 = subnet mask
255.255.255.0
```

## 3. Check Routes

```bash
ip route
```

or:

```bash
route -n
```

Example:

```text
default via 192.168.1.1
```

Meaning:

```text
Default gateway is 192.168.1.1
```

## 4. Test Connectivity

Ping another host:

```bash
ping 8.8.8.8
```

Ping domain:

```bash
ping google.com
```

Explanation:

```text
ICMP packets are used for connectivity testing.
```

Stop ping:

```text
CTRL + C
```

## 5. DNS Testing

Check DNS resolution:

```bash
dig google.com
```

Alternative:

```bash
nslookup google.com
```

Check configured DNS servers:

```bash
cat /etc/resolv.conf
```

## 6. Hostname

Check hostname:

```bash
hostname
```

Detailed info:

```bash
hostnamectl
```

Set hostname:

```bash
hostnamectl set-hostname server1.example.com
```

## 7. NMCLI

NetworkManager command line tool.

Show connections:

```bash
nmcli con show
```

Show devices:

```bash
nmcli dev status
```

## 8. Configure Static IP

Example:

```bash
nmcli con mod eth0 ipv4.addresses 192.168.1.100/24
```

Set gateway:

```bash
nmcli con mod eth0 ipv4.gateway 192.168.1.1
```

Set DNS:

```bash
nmcli con mod eth0 ipv4.dns 8.8.8.8
```

Set manual mode:

```bash
nmcli con mod eth0 ipv4.method manual
```

Restart connection:

```bash
nmcli con down eth0
nmcli con up eth0
```

## 9. Check Listening Ports

```bash
ss -tulpn
```

Explanation:

```text
-t = TCP
-u = UDP
-l = listening
-p = process
-n = numeric ports
```

Example:

```text
:22 = SSH
:80 = HTTP
:443 = HTTPS
```

## 10. Check Open Connections

```bash
ss -tan
```

Check specific port:

```bash
ss -tulpn | grep :80
```

## 11. Firewall Basics

Show firewall rules:

```bash
firewall-cmd --list-all
```

Open HTTP service:

```bash
firewall-cmd --add-service=http --permanent
```

Reload firewall:

```bash
firewall-cmd --reload
```

Open custom port:

```bash
firewall-cmd --add-port=8080/tcp --permanent
```

## 12. SSH

Connect to remote server:

```bash
ssh user@192.168.1.10
```

SSH runs on:

```text
TCP port 22
```

## 13. Useful Files

```text
/etc/hosts
/etc/resolv.conf
/etc/hostname
```

## 14. Troubleshooting Workflow

Check interface:

```bash
ip a
```

Check routes:

```bash
ip route
```

Check DNS:

```bash
dig google.com
```

Check connectivity:

```bash
ping 8.8.8.8
```

Check listening ports:

```bash
ss -tulpn
```

Check firewall:

```bash
firewall-cmd --list-all
```

## 15. Common Problems

### Problem:
Can ping IP but not domain.

Possible issue:

```text
DNS problem
```

### Problem:
Cannot connect to website.

Possible issues:

- firewall
- service stopped
- SELinux
- wrong port
- DNS issue

## 16. Important RHCSA Commands

```bash
ip a
ip route
ping
dig
nmcli
ss -tulpn
firewall-cmd
hostnamectl
```

## Practice Tasks

1. Check your IP address.
2. Configure a static IP.
3. Configure DNS server.
4. Test connectivity to Google DNS.
5. Open port 8080 in firewalld.
6. Verify listening ports.
7. Change hostname.
8. Test DNS resolution.
