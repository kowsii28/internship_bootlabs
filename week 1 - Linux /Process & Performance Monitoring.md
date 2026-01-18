#  3. Process & Performance Monitoring

- Linux servers run 24×7 without GUI.
- Along with core administration tasks, I’m familiar with commonly used Linux commands for file navigation, log monitoring, disk inspection, and basic networking.

  
## 3.1 Process Monitoring


| Command | Description |
|--------|-------------|
| `top`  | Displays a real-time view of running processes and system resource usage. |
| `htop` | Interactive process manager with a user-friendly interface and visual metrics. |
| `ps aux` | Displays a snapshot of all running processes across all users. |

```

top  → live view
htop → interactive
ps   → snapshot

```
---

## 3.2 Memory & CPU Monitoring

- I used `free` to check memory usage and `vmstat` to monitor CPU, memory, and I/O activity.
  

| Command | Purpose |
|--------|---------|
| `free -h` | Displays RAM usage in a human-readable format (used, free, available memory). |
| `vmstat` | Shows CPU usage, memory statistics, I/O activity, and system performance summary. |

```

free   → memory status
vmstat → overall system health

```
---

 ## 3.3 Uptime & Load Average

- If the load average exceeds the number of CPU cores, the system is overloaded.

  ```
  Load average ≈ number of processes waiting for CPU
  
  ```
  
### Command

```
uptime

```

Load Average Meaning
- Average number of processes waiting for CPU

| Load Average | Meaning |
|-------------|---------|
| `< CPU cores` | System is healthy and has spare CPU capacity |
| `= CPU cores` | CPU is fully utilized |
| `> CPU cores` | System is overloaded and processes are waiting |

---
## Additional Important Linux Commands

These commands are commonly used in real Linux environments and were understood at a basic level during the internship.

### File & Navigation
| Command | Purpose |
|-------|--------|
| `pwd` | Displays the current working directory |
| `cd` | Changes the current directory |
| `ls -l` | Lists files with permissions and ownership |
| `ls -a` | Shows hidden files |


### Log Monitoring & Search
| Command | Purpose |
|-------|--------|
| `tail -f logfile` | Monitors logs in real time |
| `grep keyword file` | Searches for a keyword inside files |


### Disk & Mounting
| Command | Purpose |
|-------|--------|
| `mount` | Displays mounted filesystems |
| `lsblk` | Shows disk and partition structure |


### Networking Basics
| Command | Purpose |
|-------|--------|
| `ip a` | Displays network interface details |
| `ping` | Tests network connectivity |
| `ssh` | Secure remote login to servers |
