# Common Linux Commands

Essential Linux commands every user should know.

## 📁 File and Directory Operations

### Navigation
```bash
# Print working directory
pwd

# List files and directories
ls                  # Basic list
ls -l               # Long format with details
ls -la              # Include hidden files
ls -lh              # Human-readable file sizes
ls -lt              # Sort by modification time
ls -lS              # Sort by file size

# Change directory
cd /path/to/dir     # Absolute path
cd ..               # Parent directory
cd ~                # Home directory
cd -                # Previous directory

# Create directory
mkdir dirname
mkdir -p path/to/nested/dir  # Create parent directories
```

### File Operations
```bash
# Create empty file or update timestamp
touch filename.txt

# Copy files
cp source.txt dest.txt
cp -r source_dir/ dest_dir/     # Copy directory recursively
cp -p file.txt backup.txt       # Preserve permissions/timestamps
cp -i file.txt dest.txt         # Interactive (prompt before overwrite)

# Move/Rename files
mv oldname.txt newname.txt
mv file.txt /path/to/destination/
mv -i file.txt dest.txt         # Interactive mode

# Remove files
rm file.txt
rm -r directory/                # Remove directory recursively
rm -f file.txt                  # Force removal without prompt
rm -rf directory/               # Force recursive removal (DANGEROUS!)

# Create symbolic link
ln -s /path/to/file linkname
```

### File Viewing and Editing
```bash
# View file contents
cat file.txt                    # Display entire file
cat file1.txt file2.txt         # Concatenate files
tac file.txt                    # Display in reverse order

# View file page by page
less file.txt                   # Navigate with arrows, q to quit
more file.txt                   # Similar to less (older)

# View first/last lines
head file.txt                   # First 10 lines
head -n 20 file.txt             # First 20 lines
tail file.txt                   # Last 10 lines
tail -n 20 file.txt             # Last 20 lines
tail -f /var/log/syslog         # Follow file in real-time

# Count lines, words, characters
wc file.txt                     # Lines, words, characters
wc -l file.txt                  # Count lines only
wc -w file.txt                  # Count words only
```

## 🔍 Search and Find

### Find Files
```bash
# Find by name
find /path -name "filename.txt"
find . -name "*.log"                    # Wildcard search
find /home -iname "FILE.TXT"            # Case-insensitive

# Find by type
find /path -type f                      # Files only
find /path -type d                      # Directories only

# Find by size
find /path -size +100M                  # Larger than 100MB
find /path -size -1M                    # Smaller than 1MB

# Find by modification time
find /path -mtime -7                    # Modified in last 7 days
find /path -mtime +30                   # Modified more than 30 days ago

# Find and execute command
find /path -name "*.tmp" -delete        # Delete .tmp files
find /path -type f -exec chmod 644 {} \;

# Locate (faster, uses database)
locate filename
updatedb                                # Update locate database
```

### Search File Contents
```bash
# grep - search text in files
grep "pattern" file.txt
grep -i "pattern" file.txt              # Case-insensitive
grep -r "pattern" /path/                # Recursive search
grep -n "pattern" file.txt              # Show line numbers
grep -v "pattern" file.txt              # Invert match (exclude pattern)
grep -c "pattern" file.txt              # Count matches
grep -l "pattern" *.txt                 # Show filenames only
grep -E "regex" file.txt                # Extended regex
grep -A 3 "pattern" file.txt            # Show 3 lines after match
grep -B 3 "pattern" file.txt            # Show 3 lines before match
grep -C 3 "pattern" file.txt            # Show 3 lines before and after
```

## 📊 File Permissions and Ownership

```bash
# View permissions
ls -l file.txt
# Output: -rw-r--r-- 1 user group 1234 Nov 16 10:00 file.txt
#         ^^^^^^^^^ permissions
#         ^         file type (- = file, d = directory, l = link)
#          ^^^      owner permissions (rwx)
#             ^^^   group permissions (rwx)
#                ^^^ other permissions (rwx)

# Change permissions
chmod 755 file.txt                      # rwxr-xr-x
chmod 644 file.txt                      # rw-r--r--
chmod u+x script.sh                     # Add execute for user
chmod g-w file.txt                      # Remove write for group
chmod a+r file.txt                      # Add read for all
chmod -R 755 directory/                 # Recursive

# Permission values
# r (read)    = 4
# w (write)   = 2
# x (execute) = 1
# 7 = rwx, 6 = rw-, 5 = r-x, 4 = r--, 0 = ---

# Change ownership
chown user file.txt
chown user:group file.txt
chown -R user:group directory/

# Change group
chgrp groupname file.txt
```

## 💾 Disk Usage

```bash
# Disk space
df -h                                   # Human-readable format
df -T                                   # Show filesystem type
df /path                                # Specific filesystem

# Directory size
du -sh /path                            # Summary, human-readable
du -h --max-depth=1                     # Size of subdirectories
du -sh *                                # Size of all items in current dir

# Largest files/directories
du -ah /path | sort -rh | head -20      # Top 20 largest
```

## 🔄 Process Management

```bash
# View processes
ps aux                                  # All processes
ps aux | grep processname               # Find specific process
ps -ef                                  # Alternative format
pgrep processname                       # Find PID by name

# Top/htop
top                                     # Interactive process viewer
htop                                    # Enhanced version (if installed)

# Kill processes
kill PID                                # Terminate process
kill -9 PID                             # Force kill
killall processname                     # Kill by name
pkill processname                       # Similar to killall

# Background/Foreground
command &                               # Run in background
jobs                                    # List background jobs
fg %1                                   # Bring job 1 to foreground
bg %1                                   # Resume job 1 in background
Ctrl+Z                                  # Suspend current process

# Process priority
nice -n 10 command                      # Start with lower priority
renice -n 5 -p PID                      # Change priority of running process
```

## 📦 Archive and Compression

```bash
# tar - Archive files
tar -cvf archive.tar files/             # Create tar archive
tar -xvf archive.tar                    # Extract tar archive
tar -tvf archive.tar                    # List contents

# tar with compression
tar -czvf archive.tar.gz files/         # Create gzip compressed tar
tar -xzvf archive.tar.gz                # Extract gzip tar
tar -cjvf archive.tar.bz2 files/        # Create bzip2 compressed tar
tar -xjvf archive.tar.bz2               # Extract bzip2 tar

# zip/unzip
zip archive.zip file1 file2
zip -r archive.zip directory/
unzip archive.zip
unzip archive.zip -d /destination/

# gzip/gunzip
gzip file.txt                           # Creates file.txt.gz
gunzip file.txt.gz                      # Extracts to file.txt
```

## 🌐 Network Commands

```bash
# Network configuration
ip addr show                            # Show IP addresses
ip link show                            # Show network interfaces
ifconfig                                # Legacy command

# Test connectivity
ping google.com
ping -c 4 8.8.8.8                       # Send 4 packets

# DNS lookup
nslookup google.com
dig google.com
host google.com

# Download files
wget https://example.com/file.txt
wget -O custom_name.txt https://url
curl -O https://example.com/file.txt
curl -L https://url                     # Follow redirects

# Network statistics
netstat -tuln                           # Listening ports
ss -tuln                                # Modern alternative
netstat -an | grep ESTABLISHED          # Active connections
```

## 🔧 System Information

```bash
# System info
uname -a                                # All system info
uname -r                                # Kernel version
hostname                                # System hostname
uptime                                  # System uptime
whoami                                  # Current user
id                                      # User and group IDs

# Hardware info
lscpu                                   # CPU info
lsmem                                   # Memory info
lsblk                                   # Block devices
lspci                                   # PCI devices
lsusb                                   # USB devices
dmidecode                               # Hardware details (requires root)

# Memory and CPU
free -h                                 # Memory usage
cat /proc/cpuinfo                       # Detailed CPU info
cat /proc/meminfo                       # Detailed memory info
```

## 📝 Text Processing

```bash
# Sort
sort file.txt                           # Sort alphabetically
sort -n file.txt                        # Sort numerically
sort -r file.txt                        # Reverse sort
sort -u file.txt                        # Sort and remove duplicates

# Unique
uniq file.txt                           # Remove adjacent duplicates
sort file.txt | uniq                    # Sort then remove duplicates
uniq -c file.txt                        # Count occurrences

# Cut
cut -d',' -f1 file.csv                  # Extract 1st field (comma delimiter)
cut -c1-10 file.txt                     # Extract characters 1-10

# Awk
awk '{print $1}' file.txt               # Print first column
awk -F',' '{print $2}' file.csv         # Custom delimiter

# Sed
sed 's/old/new/' file.txt               # Replace first occurrence
sed 's/old/new/g' file.txt              # Replace all occurrences
sed -i 's/old/new/g' file.txt           # In-place replacement
sed -n '10,20p' file.txt                # Print lines 10-20
```

## 🔗 Redirection and Pipes

```bash
# Output redirection
command > file.txt                      # Overwrite file
command >> file.txt                     # Append to file
command 2> error.log                    # Redirect stderr
command &> output.log                   # Redirect both stdout and stderr
command > /dev/null                     # Discard output

# Input redirection
command < input.txt

# Pipes
command1 | command2                     # Pipe output to another command
ps aux | grep httpd | wc -l             # Count httpd processes
cat file.txt | sort | uniq              # Sort and remove duplicates
```

## 🔑 Useful Shortcuts

```bash
Ctrl+C      # Kill current process
Ctrl+Z      # Suspend current process
Ctrl+D      # Exit/logout
Ctrl+L      # Clear screen
Ctrl+A      # Move to beginning of line
Ctrl+E      # Move to end of line
Ctrl+U      # Delete from cursor to beginning
Ctrl+K      # Delete from cursor to end
Ctrl+R      # Search command history
!!          # Repeat last command
!$          # Last argument of previous command
```

## Related Topics

- [[Linux File System]]
- [[Linux Permissions]]
- [[Bash Scripting Basics]]
- [[Cheat Sheets/Linux Commands|Linux Commands Cheat Sheet]]

---

#linux #commands #cheatsheet #sysadmin
