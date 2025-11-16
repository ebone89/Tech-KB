# Linux Commands Cheat Sheet

Quick reference for essential Linux commands.

## 📁 File Operations

```bash
# Navigation
pwd                         # Print working directory
ls -lah                     # List all files with details
cd /path/to/dir            # Change directory
cd ~                       # Home directory
cd -                       # Previous directory

# File management
touch file.txt             # Create empty file
cp source dest             # Copy file
cp -r dir1/ dir2/         # Copy directory
mv old new                 # Move/rename
rm file.txt                # Remove file
rm -rf dir/                # Remove directory recursively
mkdir -p path/to/dir      # Create nested directories
ln -s /path/file link     # Create symbolic link

# File viewing
cat file.txt               # Display file
less file.txt              # Page through file
head -n 20 file.txt       # First 20 lines
tail -f /var/log/syslog   # Follow log file
```

## 🔍 Search and Find

```bash
# Find files
find /path -name "*.log"                    # Find by name
find /path -type f -size +100M              # Find large files
find /path -mtime -7                        # Modified last 7 days
find /path -name "*.tmp" -delete            # Find and delete

# Search content
grep -r "pattern" /path/                    # Recursive search
grep -i "pattern" file.txt                  # Case-insensitive
grep -n "pattern" file.txt                  # Show line numbers
grep -v "pattern" file.txt                  # Invert match

# Locate (faster)
locate filename
updatedb                                     # Update locate database
```

## 👤 Permissions and Ownership

```bash
# View permissions
ls -l file.txt

# Change permissions
chmod 755 file.txt                          # rwxr-xr-x
chmod u+x script.sh                         # Add execute for user
chmod -R 755 dir/                           # Recursive

# Change ownership
chown user:group file.txt
chown -R user:group dir/

# Permission values: r=4, w=2, x=1
# 755 = rwxr-xr-x, 644 = rw-r--r--, 777 = rwxrwxrwx
```

## 💾 Disk Usage

```bash
# Disk space
df -h                                       # Human-readable format
df -T                                       # Show filesystem type

# Directory size
du -sh /path                                # Summary
du -h --max-depth=1                         # Subdirectory sizes
du -sh * | sort -h                          # Sorted by size
```

## 🔄 Process Management

```bash
# View processes
ps aux                                      # All processes
ps aux | grep process_name
top                                         # Interactive viewer
htop                                        # Better top (if installed)

# Kill processes
kill PID                                    # Terminate
kill -9 PID                                 # Force kill
killall process_name                        # Kill by name
pkill pattern                               # Kill by pattern

# Background jobs
command &                                   # Run in background
jobs                                        # List jobs
fg %1                                       # Foreground job 1
bg %1                                       # Background job 1
```

## 🌐 Network Commands

```bash
# Network info
ip addr show                                # IP addresses
ip link show                                # Network interfaces
ifconfig                                    # Legacy command

# Connectivity
ping -c 4 google.com                        # Send 4 packets
traceroute google.com                       # Trace route
nslookup google.com                         # DNS lookup
dig google.com                              # Detailed DNS

# Download
wget https://example.com/file.txt
curl -O https://example.com/file.txt

# Network stats
netstat -tuln                               # Listening ports
ss -tuln                                    # Modern alternative
netstat -an | grep ESTABLISHED
```

## 📦 Package Management

### Debian/Ubuntu (apt)
```bash
sudo apt update                             # Update package list
sudo apt upgrade                            # Upgrade packages
sudo apt install package_name               # Install package
sudo apt remove package_name                # Remove package
sudo apt autoremove                         # Remove unused packages
sudo apt search keyword                     # Search packages
apt list --installed                        # List installed
```

### Red Hat/CentOS (yum/dnf)
```bash
sudo yum update                             # Update packages
sudo yum install package_name               # Install package
sudo yum remove package_name                # Remove package
sudo yum search keyword                     # Search packages
yum list installed                          # List installed

# dnf (newer Fedora/RHEL 8+)
sudo dnf update
sudo dnf install package_name
```

## 🗜️ Archive and Compression

```bash
# tar
tar -czvf archive.tar.gz files/             # Create gzip archive
tar -xzvf archive.tar.gz                    # Extract gzip archive
tar -cjvf archive.tar.bz2 files/            # Create bzip2 archive
tar -xjvf archive.tar.bz2                   # Extract bzip2 archive
tar -tvf archive.tar.gz                     # List contents

# zip
zip -r archive.zip directory/
unzip archive.zip

# gzip
gzip file.txt                               # Compress
gunzip file.txt.gz                          # Decompress
```

## 📝 Text Processing

```bash
# Sort and unique
sort file.txt                               # Sort lines
sort -n file.txt                            # Numeric sort
sort -r file.txt                            # Reverse sort
uniq file.txt                               # Remove duplicates
sort file.txt | uniq -c                     # Count occurrences

# Cut and paste
cut -d',' -f1 file.csv                      # Extract field
paste file1 file2                           # Merge files

# Awk
awk '{print $1}' file.txt                   # Print first column
awk -F',' '{print $2}' file.csv             # Custom delimiter

# Sed
sed 's/old/new/g' file.txt                  # Replace text
sed -i 's/old/new/g' file.txt               # In-place replace
sed -n '10,20p' file.txt                    # Print lines 10-20
```

## 🔧 System Information

```bash
# System
uname -a                                    # All system info
hostname                                    # System hostname
uptime                                      # Uptime
whoami                                      # Current user
id                                          # User/group IDs

# Hardware
lscpu                                       # CPU info
free -h                                     # Memory usage
lsblk                                       # Block devices
lspci                                       # PCI devices
lsusb                                       # USB devices

# Logs
tail -f /var/log/syslog                     # Follow system log
journalctl -f                               # Follow systemd journal
journalctl -u service_name                  # Service logs
dmesg                                       # Kernel messages
```

## 🚀 System Management

```bash
# Services (systemd)
sudo systemctl start service_name           # Start service
sudo systemctl stop service_name            # Stop service
sudo systemctl restart service_name         # Restart service
sudo systemctl status service_name          # Check status
sudo systemctl enable service_name          # Enable at boot
sudo systemctl disable service_name         # Disable at boot

# User management
sudo useradd username                       # Add user
sudo passwd username                        # Set password
sudo usermod -aG group username             # Add to group
sudo userdel username                       # Delete user

# Scheduled tasks
crontab -e                                  # Edit cron jobs
crontab -l                                  # List cron jobs
# Format: minute hour day month weekday command
# Example: 0 2 * * * /path/to/script.sh    # Run at 2 AM daily
```

## 🔗 I/O Redirection

```bash
command > file.txt                          # Overwrite file
command >> file.txt                         # Append to file
command 2> error.log                        # Redirect stderr
command &> output.log                       # Redirect both
command < input.txt                         # Input from file
command1 | command2                         # Pipe output
command > /dev/null 2>&1                    # Discard all output
```

## ⌨️ Keyboard Shortcuts

```bash
Ctrl+C          # Kill current process
Ctrl+Z          # Suspend process
Ctrl+D          # Exit/logout
Ctrl+L          # Clear screen
Ctrl+A          # Move to line start
Ctrl+E          # Move to line end
Ctrl+U          # Delete to line start
Ctrl+K          # Delete to line end
Ctrl+R          # Search history
!!              # Repeat last command
!$              # Last argument
```

## 🔍 File Searching Patterns

```bash
# Wildcards
*               # Match any characters
?               # Match single character
[abc]           # Match a, b, or c
[0-9]           # Match any digit
[!0-9]          # Match non-digit

# Examples
ls *.txt                                    # All .txt files
rm file?.txt                                # file1.txt, file2.txt, etc.
ls [A-Z]*                                   # Files starting with uppercase
```

## 💡 Useful One-Liners

```bash
# Find largest files
find / -type f -size +100M 2>/dev/null | head -10

# Find and delete old files
find /tmp -type f -mtime +30 -delete

# Monitor disk space
watch -n 5 df -h

# Count files in directory
find /path -type f | wc -l

# Find files modified today
find /path -type f -mtime 0

# Replace text in multiple files
find . -name "*.txt" -exec sed -i 's/old/new/g' {} \;

# Find broken symbolic links
find -L /path -type l

# List top 10 largest directories
du -h /path | sort -rh | head -10

# Monitor log in real-time with filtering
tail -f /var/log/syslog | grep -i error

# Check open ports
sudo lsof -i -P -n | grep LISTEN
```

## Related Topics

- [[Linux/Common Commands|Common Commands (Detailed)]]
- [[Linux/Bash Scripting Basics|Bash Scripting]]
- [[Linux/System Monitoring|System Monitoring]]

---

#linux #commands #cheatsheet #reference
