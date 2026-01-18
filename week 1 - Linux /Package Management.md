# 4. Package Management

- Software installation and updates must be secure and consistent

# Package Management: Ubuntu vs Red Hat 

## Core Idea

Linux is **not a single operating system**.  
Linux is a **kernel**.

Ubuntu and Red Hat are **different Linux distributions** built on top of the Linux kernel.  
Because they are developed by **different organizations**, they follow **different software ecosystems**.

That is why:
- Ubuntu uses **APT**
- Red Hat uses **YUM / DNF**
- Ubuntu is a Debian-based Linux distribution that uses APT and .deb packages.
- Red Hat is an enterprise Linux distribution that uses RPM packages managed by YUM or DNF.
- They use different package managers because they belong to different Linux ecosystems.

---

## Ubuntu (Debian-Based Linux)


- Ubuntu is a **Linux distribution**
- It is based on **Debian**
- Maintained by **Canonical**
- Focuses on:
  - Ease of use
  - Developers
  - Cloud environments
  - Beginners and learning systems

Ubuntu is commonly used in:
- Cloud platforms (AWS, Azure, GCP)
- Startups
- Education and internships

---

### Why Ubuntu Uses APT

Ubuntu follows the **Debian ecosystem**, which uses:
- `.deb` package format
- `dpkg` as the low-level package tool
- `apt` as the high-level package manager

**APT = Advanced Package Tool**

APT is designed to:

- Automatically resolve dependencies
- Be simple and user-friendly
- Support frequent updates

---

### Ubuntu Package Commands

```

apt update
apt install package_name
apt upgrade
apt remove package_name
apt purge package_name

```

## What is a `.deb` Package?

A `.deb` file is a **software package format** used by **Debian-based Linux systems** such as Ubuntu.

It contains:
- Program binaries
- Dependency information
- Configuration scripts


---

## Red Hat (Enterprise Linux)

### What Red Hat Is

Red Hat Enterprise Linux (RHEL) is a **commercial Linux distribution**.

- Developed by **Red Hat (IBM)**
- Focuses on:
  - Stability
  - Security
  - Enterprise servers
  - Long-term support

Red Hat systems are commonly used in:
- Banks
- Large enterprises
- Data centers
- Mission-critical servers

---

## Why Red Hat Uses YUM / DNF

Red Hat follows a **different Linux ecosystem**:

- Uses `.rpm` package format
- Uses an RPM package database
- Uses **YUM** or **DNF** as package managers

**RPM = Red Hat Package Manager**

---

## YUM vs DNF

### YUM
- Older package manager
- Slower dependency resolution

### DNF
- Modern replacement for YUM
- Faster and more reliable
- Better dependency handling

DNF is the **default package manager** in modern Red Hat systems.

---

## Red Hat Package Commands

```

dnf install package_name
dnf update
dnf remove package_name

```

## 4.1 Package Managers

- Linux distributions use different package managers to install, update, and remove software packages.

| Distribution | Package Manager | Package Type |
|-------------|----------------|--------------|
| Ubuntu / Debian | `apt` | `.deb` |
| Red Hat / CentOS | `yum` / `dnf` | `.rpm` |

---

## 4.2 APT Commands

### Update Package List

```

sudo apt update

```

### Install Package

```

sudo apt install package_name

```

### Upgrade Installed Packages

```

sudo apt upgrade

```

### Remove Unused Packages

```
sudo apt autoremove

```

## 4.3 Remove vs Purge


### Difference Between `apt remove` and `apt purge`

| Command | What it Does | Configuration Files | When to Use |
|--------|--------------|---------------------|-------------|
| `apt remove package_name` | Uninstalls the software package from the system | ❌ Configuration files are **kept** | Use when you may reinstall the package later and want to keep previous settings |
| `apt purge package_name` | Completely removes the software package from the system | ✅ Configuration files are **deleted** | Use when you want a clean removal with no leftover settings |

---

## 4.4 Package Information Commands

These commands are used to **inspect installed packages, check updates, and search software** in Ubuntu using APT.


### Package Information Commands 

| Command | Purpose | When to Use |
|--------|--------|------------|
| `apt show package_name` | Displays detailed information about a package such as version, description, dependencies, and size | Use when you want to understand what a package does before installing |
| `apt list --installed` | Lists all packages currently installed on the system | Use to know about installed software or verify if a package is already installed |
| `apt list --upgradable` | Shows packages that have newer versions available for upgrade | Use before upgrading to know what will be updated |
| `apt search keyword` | Searches available packages based on a keyword | Use when you don’t know the exact package name |

---

### Example 

```

apt show nginx
apt list --installed
apt list --upgradable
apt search python

```

### Why Ubuntu and Red Hat Use Different Package Managers

| Aspect | Ubuntu / Debian | Red Hat |
|-------|----------------|---------|
| Package format | `.deb` | `.rpm` |
| Package manager | `apt` | `yum` / `dnf` |
| Ecosystem | Debian-based | RPM-based |
| Focus | Ease of use, cloud, development | Enterprise stability |
| Maintained by | Canonical | Red Hat (IBM) |
