# 5. Automation & Services

- Manual work doesn’t scale. Automation is mandatory.
- During my internship, I worked on Linux system administration including user and permission management, storage and memory handling, process monitoring, package management, and automation using cron and systemd.

## 5.1 Cron Jobs

### Concept

- Cron = time-based job scheduler

#### Example

```

0 * * * * /home/kowsalya/disk.sh >> cron.log 2>&1

```

- Runs every hour.

## 5.2 Disk Monitoring Script (Shell)

### Purpose

- Monitor disk usage

- Alert when usage exceeds threshold

**Cron ensures:**

        - No manual intervention

        - Prevents disk-full crashes


## 5.3 systemd Services

**Why systemd**

- Run programs at boot

- Manage background services

- Auto-restart & logging

**Steps**

```

sudo vi /etc/systemd/system/new.service
sudo chmod 755 new.service
sudo systemctl daemon-reload
sudo systemctl enable new.service
sudo systemctl start new.service

```

**Service Management**

```

systemctl status new.service
systemctl stop new.service
systemctl restart new.service
systemctl disable new.service

```
