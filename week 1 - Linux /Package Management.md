# 4. Package Management

- Software installation and updates must be secure and consistent


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
