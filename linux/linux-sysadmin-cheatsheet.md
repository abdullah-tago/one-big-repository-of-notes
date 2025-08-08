# Linux System Administration Cheatsheet

A comprehensive reference guide for Linux system administration tasks.

## Table of Contents
- [File and Directory Operations](#file-and-directory-operations)
- [Process Management](#process-management)
- [System Monitoring](#system-monitoring)
- [Network Management](#network-management)
- [User Management](#user-management)
- [Service Management](#service-management)
- [Log Management](#log-management)
- [Disk Management](#disk-management)
- [Package Management](#package-management)
- [System Information](#system-information)
- [Archive and Compression](#archive-and-compression)
- [Text Processing](#text-processing)
- [Permissions](#permissions)

## File and Directory Operations

### Basic Operations
```bash
# List files and directories
ls -la                    # Detailed list with hidden files
ls -lh                    # Human readable file sizes
ls -lt                    # Sort by modification time

# Change directory
cd /path/to/directory     # Change to specific directory
cd ..                     # Go up one directory
cd ~                      # Go to home directory
cd -                      # Go to previous directory

# Create directories
mkdir directory_name      # Create single directory
mkdir -p path/to/nested   # Create nested directories

# Copy files and directories
cp file1 file2           # Copy file
cp -r dir1 dir2          # Copy directory recursively
cp -a source dest        # Archive mode (preserve attributes)

# Move/rename files
mv oldname newname       # Rename file/directory
mv file /path/to/dest    # Move file to destination

# Remove files and directories
rm file                  # Remove file
rm -r directory          # Remove directory recursively
rm -f file               # Force remove without prompt
rm -rf directory         # Force remove directory (dangerous!)

# Find files
find /path -name "*.txt"        # Find files by name
find /path -type f -size +100M  # Find files larger than 100MB
find /path -mtime -7            # Find files modified in last 7 days
locate filename                 # Quick file search (requires updatedb)
</bash>
```

### File Content Operations
```bash
# View file contents
cat file                 # Display entire file
less file                # View file page by page
head -n 10 file         # Show first 10 lines
tail -n 10 file         # Show last 10 lines
tail -f file            # Follow file changes in real-time

# Create/edit files
touch filename          # Create empty file or update timestamp
nano filename           # Edit with nano editor
vim filename            # Edit with vim editor
</bash>
```

## Process Management

### Viewing Processes
```bash
# List processes
ps aux                   # Show all processes
ps -ef                   # Alternative format
pstree                   # Show process tree
top                      # Real-time process monitor
htop                     # Enhanced process monitor (if installed)

# Find specific processes
ps aux | grep process_name
pgrep process_name       # Get PID by process name
pidof process_name       # Alternative to get PID
</bash>
```

### Controlling Processes
```bash
# Kill processes
kill PID                 # Terminate process by PID
kill -9 PID             # Force kill process
killall process_name     # Kill all processes by name
pkill process_name       # Kill processes by name pattern

# Background and foreground
command &               # Run command in background
jobs                    # List background jobs
fg %1                   # Bring job 1 to foreground
bg %1                   # Send job 1 to background
nohup command &         # Run command immune to hangups
</bash>
```

## System Monitoring

### System Resources
```bash
# CPU and memory usage
top                      # Real-time system monitor
htop                     # Enhanced system monitor
free -h                  # Memory usage (human readable)
vmstat 1                 # Virtual memory statistics every second

# Disk usage
df -h                    # Disk space usage (human readable)
du -sh /path            # Directory size summary
du -sh * | sort -hr     # Sort directories by size
iotop                   # Monitor disk I/O by process

# System load
uptime                  # System uptime and load average
w                       # Who is logged in and what they're doing
who                     # Show logged in users
</bash>
```

### Real-time Monitoring
```bash
# Watch command output
watch -n 2 command      # Run command every 2 seconds
watch "df -h"           # Monitor disk space
watch "free -m"         # Monitor memory usage

# System calls and library calls
strace command          # Trace system calls
ltrace command          # Trace library calls
</bash>
```

## Network Management

### Network Configuration
```bash
# Interface information
ip addr show            # Show IP addresses
ip link show            # Show network interfaces
ifconfig               # Alternative (deprecated)

# Routing
ip route show          # Show routing table
route -n               # Alternative routing table
ping hostname          # Test connectivity
traceroute hostname    # Trace network path

# Network statistics
netstat -tuln          # Show listening ports
ss -tuln               # Modern alternative to netstat
ss -tulpn              # Include process information
lsof -i :port          # Show what's using a specific port
</bash>
```

### Network Tools
```bash
# Download/upload
wget URL               # Download file from URL
curl URL               # Transfer data from/to server
scp file user@host:/path  # Secure copy over SSH
rsync -av source dest  # Synchronize files/directories

# DNS
nslookup hostname      # DNS lookup
dig hostname           # DNS lookup (more detailed)
host hostname          # Simple DNS lookup
</bash>
```

## User Management

### User Operations
```bash
# User information
whoami                 # Current username
id                     # Current user ID and groups
groups username        # Show user groups
finger username        # User information (if available)

# User management (requires sudo)
sudo useradd username       # Add new user
sudo userdel username       # Delete user
sudo usermod -aG group user # Add user to group
sudo passwd username        # Change user password
su - username              # Switch to another user
sudo -u username command   # Run command as another user
</bash>
```

### Group Management
```bash
# Group operations
groups                 # Show current user groups
sudo groupadd groupname    # Create new group
sudo groupdel groupname    # Delete group
sudo gpasswd -a user group # Add user to group
sudo gpasswd -d user group # Remove user from group
</bash>
```

## Service Management

### Systemd (Modern Linux)
```bash
# Service status
systemctl status service_name    # Show service status
systemctl list-units --type=service  # List all services

# Service control
sudo systemctl start service_name    # Start service
sudo systemctl stop service_name     # Stop service
sudo systemctl restart service_name  # Restart service
sudo systemctl reload service_name   # Reload configuration

# Service management
sudo systemctl enable service_name   # Enable service at boot
sudo systemctl disable service_name  # Disable service at boot
sudo systemctl mask service_name     # Mask service (prevent start)
sudo systemctl unmask service_name   # Unmask service
</bash>
```

### Legacy Init Systems
```bash
# SysV init
sudo service service_name start     # Start service
sudo service service_name stop      # Stop service
sudo service service_name restart   # Restart service
sudo service service_name status    # Check status

# Check runlevels
runlevel               # Current runlevel
sudo update-rc.d service_name enable   # Enable service (Debian/Ubuntu)
sudo chkconfig service_name on         # Enable service (Red Hat/CentOS)
</bash>
```

## Log Management

### System Logs
```bash
# Journal logs (systemd)
journalctl                    # View all journal logs
journalctl -f                 # Follow logs in real-time
journalctl -u service_name    # Logs for specific service
journalctl --since "1 hour ago"  # Recent logs
journalctl --until "2021-01-01"  # Logs until date

# Traditional log files
tail -f /var/log/syslog      # Follow system log
tail -f /var/log/messages    # Follow messages (Red Hat)
tail -f /var/log/auth.log    # Follow authentication log
less /var/log/dmesg          # Kernel messages
</bash>
```

### Log Analysis
```bash
# Search logs
grep "error" /var/log/syslog     # Search for errors
zgrep "pattern" /var/log/*.gz    # Search compressed logs
awk '/pattern/ {print $1, $2}' /var/log/file  # Extract specific fields

# Log rotation
sudo logrotate -f /etc/logrotate.conf  # Force log rotation
</bash>
```

## Disk Management

### Disk Information
```bash
# Disk usage
df -h                    # Filesystem disk space usage
du -sh directory         # Directory size
lsblk                    # List block devices
fdisk -l                 # List disk partitions (requires sudo)
blkid                    # Show block device UUIDs
</bash>
```

### Mounting and Unmounting
```bash
# Mount operations
sudo mount /dev/sdb1 /mnt/usb    # Mount device
sudo umount /mnt/usb             # Unmount device
mount                            # Show mounted filesystems
cat /proc/mounts                 # Alternative to show mounts

# Persistent mounts
sudo nano /etc/fstab             # Edit filesystem table
sudo mount -a                    # Mount all fstab entries
</bash>
```

### Disk Operations
```bash
# Check filesystem
sudo fsck /dev/sdb1              # Check filesystem
sudo fsck -f /dev/sdb1           # Force check

# Create filesystem
sudo mkfs.ext4 /dev/sdb1         # Create ext4 filesystem
sudo mkfs.ntfs /dev/sdb1         # Create NTFS filesystem

# Disk usage analysis
ncdu /path                       # Interactive disk usage (if installed)
</bash>
```

## Package Management

### Debian/Ubuntu (APT)
```bash
# Update package lists
sudo apt update                  # Update package lists
sudo apt upgrade                 # Upgrade packages
sudo apt full-upgrade           # Full system upgrade

# Package operations
sudo apt install package_name   # Install package
sudo apt remove package_name    # Remove package
sudo apt purge package_name     # Remove package and config files
sudo apt autoremove             # Remove unused dependencies

# Package information
apt search keyword              # Search for packages
apt show package_name           # Show package information
apt list --installed           # List installed packages
dpkg -l                        # Alternative package list
</bash>
```

### Red Hat/CentOS (YUM/DNF)
```bash
# Package operations (YUM - older systems)
sudo yum update                 # Update packages
sudo yum install package_name   # Install package
sudo yum remove package_name    # Remove package
yum search keyword             # Search packages
yum info package_name          # Package information

# Package operations (DNF - newer systems)
sudo dnf update                # Update packages
sudo dnf install package_name  # Install package
sudo dnf remove package_name   # Remove package
dnf search keyword            # Search packages
dnf info package_name         # Package information
</bash>
```

## System Information

### Hardware Information
```bash
# CPU information
lscpu                   # CPU architecture info
cat /proc/cpuinfo      # Detailed CPU information
nproc                  # Number of processing units

# Memory information
free -h                # Memory usage
cat /proc/meminfo      # Detailed memory information

# Hardware details
lshw                   # List hardware (requires sudo)
lspci                  # PCI devices
lsusb                  # USB devices
dmidecode              # DMI/SMBIOS information (requires sudo)
</bash>
```

### System Information
```bash
# System details
uname -a               # All system information
hostnamectl            # System hostname and related info
cat /etc/os-release    # Operating system information
lsb_release -a         # Distribution information
uptime                 # System uptime
date                   # Current date and time
timedatectl            # Time and date settings

# Kernel information
uname -r               # Kernel version
cat /proc/version      # Kernel version details
modinfo module_name    # Module information
lsmod                  # List loaded modules
</bash>
```

## Archive and Compression

### TAR Archives
```bash
# Create archives
tar -cvf archive.tar files/     # Create tar archive
tar -czvf archive.tar.gz files/ # Create compressed archive (gzip)
tar -cjvf archive.tar.bz2 files/ # Create compressed archive (bzip2)

# Extract archives
tar -xvf archive.tar            # Extract tar archive
tar -xzvf archive.tar.gz        # Extract gzipped archive
tar -xjvf archive.tar.bz2       # Extract bzip2 archive

# List archive contents
tar -tvf archive.tar            # List contents without extracting
</bash>
```

### Compression
```bash
# Compress files
gzip file               # Compress with gzip
gunzip file.gz          # Decompress gzip
bzip2 file              # Compress with bzip2
bunzip2 file.bz2        # Decompress bzip2
zip archive.zip files   # Create zip archive
unzip archive.zip       # Extract zip archive
</bash>
```

## Text Processing

### Text Viewing and Editing
```bash
# View text
cat file                # Display entire file
less file               # View file page by page
more file               # View file page by page (basic)
head -n 10 file        # First 10 lines
tail -n 10 file        # Last 10 lines

# Search in files
grep pattern file       # Search for pattern
grep -r pattern dir     # Recursive search
grep -i pattern file    # Case-insensitive search
grep -v pattern file    # Invert match (exclude pattern)
</bash>
```

### Text Processing Tools
```bash
# Text manipulation
sort file               # Sort lines
uniq file               # Remove duplicate lines
wc file                 # Word, line, character count
cut -d',' -f1 file     # Extract first field (CSV)
awk '{print $1}' file   # Print first column
sed 's/old/new/g' file  # Replace text
tr 'a-z' 'A-Z' < file  # Translate characters (lowercase to uppercase)
</bash>
```

## Permissions

### File Permissions
```bash
# View permissions
ls -l file              # Long listing with permissions
stat file               # Detailed file information

# Change permissions
chmod 755 file          # Set permissions (rwxr-xr-x)
chmod u+x file          # Add execute permission for user
chmod g-w file          # Remove write permission for group
chmod o=r file          # Set only read permission for others

# Change ownership
sudo chown user:group file    # Change owner and group
sudo chown user file          # Change owner only
sudo chgrp group file         # Change group only
</bash>
```

### Special Permissions
```bash
# SUID, SGID, Sticky bit
chmod 4755 file         # Set SUID bit
chmod 2755 directory    # Set SGID bit
chmod 1755 directory    # Set sticky bit

# Default permissions
umask                   # Show current umask
umask 022               # Set umask
</bash>
```

## Emergency and Recovery

### System Recovery
```bash
# Boot issues
sudo grub-update        # Update GRUB bootloader
sudo update-grub        # Alternative for Debian/Ubuntu

# File system check
sudo fsck /dev/sda1     # Check filesystem
sudo fsck -f /dev/sda1  # Force check

# Emergency shell
sudo su -               # Root shell
sudo bash               # Root bash shell
</bash>
```

### System Rescue
```bash
# Mount root filesystem read-only
mount -o remount,ro /   # Remount root as read-only
mount -o remount,rw /   # Remount root as read-write

# Reset user password
sudo passwd username    # Reset password for user
</bash>
```

---

## Quick Reference Card

### Essential Commands
| Command | Description |
|---------|-------------|
| `ls -la` | List files with details |
| `cd /path` | Change directory |
| `cp -r src dst` | Copy directory |
| `rm -rf dir` | Remove directory |
| `sudo command` | Run as administrator |
| `man command` | Show manual page |
| `which command` | Show command location |
| `history` | Show command history |
| `clear` | Clear terminal |
| `exit` | Exit shell |

### Keyboard Shortcuts
| Shortcut | Description |
|----------|-------------|
| `Ctrl+C` | Interrupt/stop current command |
| `Ctrl+Z` | Suspend current command |
| `Ctrl+D` | End of file/exit |
| `Ctrl+L` | Clear screen |
| `Ctrl+R` | Search command history |
| `Tab` | Auto-complete |
| `!!` | Repeat last command |
| `!string` | Repeat last command starting with string |

### File Permission Notation
| Permission | Numeric | Symbolic |
|------------|---------|----------|
| No permission | 0 | --- |
| Execute only | 1 | --x |
| Write only | 2 | -w- |
| Write + Execute | 3 | -wx |
| Read only | 4 | r-- |
| Read + Execute | 5 | r-x |
| Read + Write | 6 | rw- |
| Read + Write + Execute | 7 | rwx |

---

*This cheatsheet covers the most commonly used Linux system administration commands. For detailed information about any command, use `man command_name` or `command_name --help`.*