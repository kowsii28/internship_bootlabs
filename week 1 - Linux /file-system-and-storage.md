# 2. File System & Storage Management

**Linux treats everything as a file, including disks and devices.**

---

## 2.1 Directory and File Operations


### Commands

```

mkdir foldername
touch filename
cat > filename
ls

```

| Command | Explanation |
|--------|------------|
| `mkdir foldername` | Creates a new directory (folder) in the file system to organize files. |
| `touch filename` | Creates an empty file or updates the timestamp of an existing file. |
| `cat > filename` | Creates a file and allows entering content from standard input, saving it to the file. |
| `ls` | Lists files and directories in the current directory. |


## 2.2 Viewing File Contents


| Command | Use Case |
|--------|----------|
| `cat`  | Small files |
| `less` | Large files (logs) |
| `more` | Page-wise viewing |
| `head` | Beginning of file |
| `tail` | End of file |


- less is preferred for logs because it doesn’t load the full file into memory.


## 2.3 Disk Usage Monitoring


### Commands

```

df -h
du -sh *

```

- Full disks can crash services

- Logs may stop writing


| Command | Explanation |
|--------|------------|
| `df -h` | Displays disk space usage of all mounted file systems in a human-readable format (GB/MB). |
| `du -sh *` | Shows disk usage of files and directories in the current location, summarized in a human-readable format. |


## 2.4 Swap Space Management


### Concept

- Swap is disk space used as virtual memory

- Prevents system crash when RAM is exhausted

**Types**
   - Swap partition
   - Swap file

**Creating Swap File**

```

sudo fallocate -l 2G /newswap
sudo chmod 600 /newswap
sudo mkswap /newswap
sudo swapon /newswap

```

**Persistent Swap**

```
- echo '/newswap swap swap defaults 0 0' | sudo tee -a /etc/fstab
```

### Making Swap Persistent – Summary

- This command adds the swap file entry to `/etc/fstab`.
- `/etc/fstab` is read during system boot to configure filesystems and swap.
- Adding the entry ensures the swap file is **enabled automatically after every reboot**.
- `tee` is used instead of redirection (`>>`) to safely write to `/etc/fstab` with root permissions.

- swapon → temporary
- fstab → permanent


## 2.5 LVM (Logical Volume Manager)

**Why LVM**

- Traditional partitions are rigid.
- LVM allows dynamic resizing without downtime.

**LVM Flow**

- Disk → Physical Volume → Volume Group → Logical Volume → Mount

  ### Why Formatting with `ext4` is Required in LVM

- LVM manages disk space but does **not** store files.
- A filesystem is required to organize and store data on a logical volume.
- After creating a Logical Volume, it must be formatted before use.
- `ext4` is commonly used because it is stable, supports large volumes, and provides journaling for reliability.
- Without formatting, the logical volume cannot be used to store files.

#### Benefits

      - Resize storage without reboot

      - Combine multiple disks
      
      - Used in production servers

### Common Filesystem Types 

A filesystem defines **how data is stored, organized, and accessed** on disk.  
Different filesystems are designed for **different use cases**.

---

#### `ext4` – Default & Safe (Most Common)

- Standard filesystem for most Linux distributions
- Stable, reliable, and well-tested
- Supports large files and volumes
- Includes journaling for crash recovery

**When to use:**  
General-purpose Linux systems, servers, and LVM logical volumes.

`ext4 = safe default for Linux`

---

#### `xfs` – Speed & Large Files

- Designed for high-performance workloads
- Very fast with large files and large disks
- Scales well for enterprise systems
- Journaling enabled

**Limitation:**  
Cannot easily shrink once expanded.

**When to use:**  
Databases, media servers, large storage systems.

`xfs = speed + big data`

---

#### `btrfs` – Snapshots & Advanced Features

- Modern filesystem with advanced capabilities
- Supports snapshots, copy-on-write, and built-in RAID
- Useful for backups and rollback scenarios

**Limitation:**  
More complex than ext4 and not required for simple setups.

**When to use:**  
Snapshot-based backups, advanced storage management.

`btrfs = snapshots & advanced control`

---

#### `vfat` (FAT32) – USB & Compatibility

- Simple filesystem with wide compatibility
- Works across Linux, Windows, and macOS
- Does not support permissions or journaling

**When to use:**  
USB drives, boot partitions, removable media.
 
`vfat = works everywhere`

---

#### `ntfs` – Windows Filesystem

- Default filesystem for Windows systems
- Linux can read and write using drivers
- Supports large files and partitions

**When to use:**  
Dual-boot systems or accessing Windows disks from Linux.
 
`ntfs = Windows disks`

---


