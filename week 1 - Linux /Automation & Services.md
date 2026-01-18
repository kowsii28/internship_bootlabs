# 5. Automation & Services

- Automation is a core responsibility of a Linux administrator.
- It ensures tasks run **automatically**, **reliably**, and **without manual intervention**.
- Manual work doesn’t scale. Automation is mandatory.
- During my internship, I worked on Linux system administration including user and permission management, storage and memory handling, process monitoring, package management, and automation using cron and systemd.

## 5.1 Cron Jobs


Cron is a **time-based job scheduler** in Linux.  
It allows commands or scripts to run **automatically at fixed times or intervals**.

Cron is commonly used for:
- Backups
- Disk usage monitoring
- Log cleanup
- Health checks
- Periodic reports

---

### What is Crontab?

`crontab` (cron table) is a file that stores:
- The schedule
- The command/script to run

Each user can have their own crontab.


### Crontab Time Format 

```

* * * * * command-to-execute
- - - - -
| | | | |
| | | | +-- Day of Week (0-6 or SUN-SAT, 0 or 7 is Sunday)
| | | +---- Month (1-12 or JAN-DEC)
| | +------ Day of Month (1-31)
| +-------- Hour (0-23, 24-hour format)
+---------- Minute (0-59)

```

#### Example

```

0 * * * * /home/kowsalya/disk.sh >> cron.log 2>&1

```

- Runs every hour.

## 5.2 Disk Monitoring Script (Shell)

### Purpose

- Monitor disk usage

- Alert when usage exceeds threshold

### Cron ensures:

        - No manual intervention

        - Prevents disk-full crashes


## 5.3 systemd Services


### What is systemd?

`systemd` is the **system and service manager** used in modern Linux distributions.

It is responsible for:
- Starting services during system boot
- Managing background processes (daemons)
- Restarting failed services
- Handling service dependencies
- Logging service activity


### What is a systemd Service?

A **systemd service** is a program that:
- Runs in the background
- Can start automatically at system boot (if enabled)
- Does not depend on user login

**Common examples:**
- Web servers (nginx, apache)
- Monitoring scripts
- Database services

---

## 5.3 Why `systemctl daemon-reload` is Required

### Command

```

sudo systemctl daemon-reload

```

#### What This Command Does (`systemctl daemon-reload`)

This command tells **systemd** to:

- Re-read all service files
- Update its internal configuration
- Refresh systemd’s in-memory view of service definitions

---

#### When You MUST Run `daemon-reload`

You must run this command when:

- You create a **new service file**
- You modify an **existing service file**
- You change service configurations under:

```

/etc/systemd/system/

```

- systemd loads service definitions into memory

- It does not automatically re-read service files when they are edited

- daemon-reload forces systemd to refresh its configuration

  
#### What Happens If You Don’t Run It?

- systemd will not detect your changes

- The service may fail to start

- Old configurations may still be used



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
