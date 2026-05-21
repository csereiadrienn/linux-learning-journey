# Cron Jobs and Scheduling

This file contains notes about scheduled tasks in Linux using cron and at.

## 1. What is Cron?

Cron is used to schedule recurring tasks automatically.

Examples:
- backups
- cleanup scripts
- monitoring
- log rotation

Cron service:

```bash
crond
```

## 2. Check Cron Service

Check status:

```bash
systemctl status crond
```

Start service:

```bash
systemctl start crond
```

Enable at boot:

```bash
systemctl enable crond
```

## 3. User Cron Jobs

Edit current user's cron jobs:

```bash
crontab -e
```

List cron jobs:

```bash
crontab -l
```

Remove cron jobs:

```bash
crontab -r
```

## 4. Cron Format

Structure:

```text
* * * * * command
- - - - -
| | | | |
| | | | day of week
| | | month
| | day of month
| hour
minute
```

## 5. Cron Examples

Run every day at 03:00:

```text
0 3 * * * /root/backup.sh
```

Run every hour:

```text
0 * * * * command
```

Run every 5 minutes:

```text
*/5 * * * * command
```

Run every Sunday at 02:00:

```text
0 2 * * 0 command
```

## 6. Cron Special Values

```text
*     = every value
*/5   = every 5 units
,     = multiple values
-     = range
```

Example:

```text
0 8,20 * * * command
```

Runs at:
- 08:00
- 20:00

## 7. System Cron Directories

Important locations:

```text
/etc/crontab
/etc/cron.hourly/
/etc/cron.daily/
/etc/cron.weekly/
/etc/cron.monthly/
```

## 8. Example Backup Script

Create script:

```bash
vi /root/backup.sh
```

Example:

```bash
#!/bin/bash

tar -zcvf /backup/etc.tar.gz /etc
```

Make executable:

```bash
chmod +x /root/backup.sh
```

Test manually:

```bash
/root/backup.sh
```

## 9. Redirect Output

Example:

```text
0 3 * * * /root/backup.sh >> /var/log/backup.log 2>&1
```

Explanation:

```text
>> appends output
2>&1 redirects errors
```

## 10. Cron Access Control

Allow users:

```text
/etc/cron.allow
```

Deny users:

```text
/etc/cron.deny
```

## 11. at Command

Used for one-time scheduled tasks.

Start at session:

```bash
at now + 5 minutes
```

Example command:

```bash
reboot
```

Finish with:

```text
CTRL + D
```

View jobs:

```bash
atq
```

Remove job:

```bash
atrm JOB_ID
```

## 12. Important RHCSA Commands

```bash
crontab -e
crontab -l
systemctl status crond
at
atq
atrm
```

## 13. Troubleshooting Cron Jobs

Check cron service:

```bash
systemctl status crond
```

Check logs:

```bash
journalctl -u crond
```

Verify script permissions:

```bash
ls -l /root/script.sh
```

Make executable:

```bash
chmod +x script.sh
```

Use full paths inside scripts.

Bad example:

```bash
tar -cvf backup.tar /etc
```

Better:

```bash
/usr/bin/tar -cvf /backup/backup.tar /etc
```

## 14. Common Problems

### Cron job does not run

Possible causes:
- crond stopped
- wrong permissions
- invalid cron syntax
- script not executable
- missing full paths

### Script works manually but not in cron

Usually environment/path issue.

## 15. Example Troubleshooting Workflow

Problem:

```text
Daily backup cron job stopped working.
```

Check cron service:

```bash
systemctl status crond
```

Check cron jobs:

```bash
crontab -l
```

Check script permissions:

```bash
ls -l /root/backup.sh
```

Check logs:

```bash
journalctl -u crond
```

Run script manually:

```bash
/root/backup.sh
```

## 16. Practice Tasks

1. Create backup script.
2. Schedule daily backup at 02:00.
3. Redirect output to log file.
4. Create hourly cleanup task.
5. Configure one-time task with at.
6. Verify cron service.
7. Troubleshoot failed cron job.
8. Restrict cron access for users.
