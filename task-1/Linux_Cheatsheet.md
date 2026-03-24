# LINUX CHEAT SHEET
## Essential Commands for Cybersecurity Professionals

---

## 📋 FILE & DIRECTORY MANAGEMENT

### Navigation
```bash
pwd                          # Print working directory
cd /path/to/dir             # Change directory
cd ~                        # Home directory
cd ..                       # Parent directory
cd -                        # Previous directory
ls                          # List files
ls -l                       # Long format
ls -a                       # Include hidden files
ls -la                      # Detailed with hidden
ls -lh                      # Human-readable sizes
ls -lS                      # Sort by size
ls -lt                      # Sort by modification time
find / -name filename       # Find file by name
find / -type f -name "*.txt" # Find all .txt files
locate filename             # Fast file search (uses db)
```

### File Operations
```bash
touch file.txt             # Create empty file
mkdir dirname              # Create directory
mkdir -p /path/to/dir      # Create with parents
cp source dest             # Copy file
cp -r source/ dest/        # Copy directory recursively
mv source dest             # Move/rename
rm file                    # Delete file
rm -rf dir/                # Delete directory (DANGEROUS!)
rm -i file                 # Interactive delete (asks for confirmation)
rm -v file                 # Verbose (shows what's deleted)

# Viewing Files
cat file.txt              # Display entire file
less file.txt             # Paginated view (q=quit, /=search)
more file.txt             # More paginated view
head -n 10 file.txt       # First 10 lines
tail -n 10 file.txt       # Last 10 lines
tail -f logfile           # Follow file (real-time monitoring)
wc -l file.txt            # Line count
wc -w file.txt            # Word count
wc -c file.txt            # Byte count
file filename             # Determine file type
```

---

## 🔐 PERMISSIONS & OWNERSHIP

### Understanding Permissions
```
Permission:  r w x   r w x   r w x
Position:    1 2 3   4 5 6   7 8 9
Owner:         ↑ ↑ ↑
Group:                ↑ ↑ ↑
Others:                     ↑ ↑ ↑

Value:   r(4) w(2) x(1)

Examples:
rwx------ = 700 (owner only)
rw-r--r-- = 644 (owner writes, others read)
rwxr-xr-x = 755 (owner full, others execute)
rwxrwxrwx = 777 (everyone full access - NOT RECOMMENDED)
```

### Chmod (Change Mode)
```bash
# Octal notation
chmod 644 file.txt        # rw-r--r--
chmod 755 script.sh       # rwxr-xr-x
chmod 700 private_key     # rwx------
chmod 777 file.txt        # rwxrwxrwx (not recommended)

# Symbolic notation
chmod u+x file            # Add execute for user
chmod g-w file            # Remove write for group
chmod o=r file            # Set others to read only
chmod a+r file            # Add read for all
chmod u+rwx,g+rx,o-x file # Multiple changes

# Recursive
chmod -R 755 directory/   # Change directory and contents
```

### Chown (Change Owner)
```bash
chown user file           # Change owner
chown user:group file     # Change owner and group
chown :group file         # Change group only
chown -R user:group dir/  # Recursive

# Example: Make root the owner, secure it
sudo chown root:root /etc/passwd
sudo chmod 644 /etc/passwd
```

### Chgrp (Change Group)
```bash
chgrp group file          # Change group
chgrp -R group dir/       # Recursive
```

---

## 👤 USER & GROUP MANAGEMENT

```bash
whoami                    # Current user
id                        # User ID and groups
id username               # Specific user info
groups                    # User's groups
groups username           # User's groups

# Adding users
sudo adduser newuser      # Interactive user creation
sudo useradd username     # Non-interactive (requires more options)
sudo useradd -m -s /bin/bash username  # Create with home and shell

# Adding groups
sudo addgroup newgroup    # Create group
sudo usermod -aG group user # Add user to group

# Removing users/groups
sudo deluser username     # Delete user
sudo delgroup groupname   # Delete group
sudo userdel -r username  # Delete user and home directory

# User information
cat /etc/passwd          # User database
cat /etc/shadow          # Password hashes (root only!)
cat /etc/group           # Group definitions
finger username          # User information (if installed)
w                        # Logged in users
who                      # Who is logged in
```

---

## 📦 PACKAGE MANAGEMENT (APT)

### Basic Operations
```bash
sudo apt update              # Update package lists
sudo apt upgrade             # Upgrade all packages
sudo apt dist-upgrade        # Upgrade with dependencies
sudo apt install package     # Install package
sudo apt install pkg1 pkg2   # Install multiple
sudo apt remove package      # Remove (keep config)
sudo apt purge package       # Remove (delete config)
sudo apt autoremove          # Remove unused dependencies
sudo apt clean               # Clean package cache
sudo apt autoclean           # Clean partial packages

# Searching and Information
apt search keyword           # Search packages
apt search ^keyword$         # Exact search
apt show package             # Show package info
apt policy package           # Show available versions
apt list --installed         # List installed packages
apt list --upgradable        # List upgradable packages

# Useful options
-y                          # Answer yes to all questions
--no-install-recommends     # Skip recommended packages
--only-upgrade              # Only upgrade, don't install
-s                          # Simulate (dry-run)
```

### DPKG (Low-level)
```bash
dpkg -i package.deb         # Install .deb file
dpkg -l                     # List installed packages
dpkg -l | grep keyword      # Search installed packages
dpkg -L package             # List files in package
dpkg -S /path/to/file       # Find package owning file
dpkg -r package             # Remove package
dpkg -P package             # Purge package
```

---

## 🔍 SEARCHING & FILTERING

### Grep (Global Regular Expression Print)
```bash
grep "pattern" file              # Search for pattern
grep -i "pattern" file           # Case-insensitive
grep -v "pattern" file           # Invert match (exclude)
grep -n "pattern" file           # Show line numbers
grep -c "pattern" file           # Count matches
grep -l "pattern" *.txt          # Files with pattern
grep -r "pattern" dir/           # Recursive search
grep -E "regex" file             # Extended regex
grep "^pattern" file             # Start of line
grep "pattern$" file             # End of line
grep -A 3 "pattern" file         # After (next 3 lines)
grep -B 3 "pattern" file         # Before (previous 3 lines)
grep -C 3 "pattern" file         # Context (3 lines)
```

### Find
```bash
find . -name "*.txt"             # Find by filename
find / -type f -name "*.txt"     # Find all .txt files
find / -type d -name "dirname"   # Find directories
find . -mtime -7                 # Modified within 7 days
find . -size +10M                # Larger than 10MB
find . -perm 644                 # Files with specific permissions
find . -name "*.txt" -delete      # Delete matching files
find . -name "*.log" -exec rm {} \; # Execute command on results
```

### Other Searching
```bash
locate filename              # Fast search (uses database)
which command               # Find command location
whereis command             # Find command, source, manual
type command               # Show command type/alias
```

---

## 🔗 TEXT PROCESSING

### Cut
```bash
cut -d: -f1 /etc/passwd    # First field (delimiter :)
cut -d: -f1,3 /etc/passwd  # Fields 1 and 3
cut -c1-5 file.txt         # First 5 characters
cut -c1,3,5 file.txt       # Characters 1, 3, 5
```

### Sort & Uniq
```bash
sort file.txt              # Sort lines
sort -r file.txt           # Reverse sort
sort -n file.txt           # Numeric sort
sort -u file.txt           # Unique sort
uniq file.txt              # Remove consecutive duplicates
uniq -c file.txt           # Count occurrences
uniq -d file.txt           # Show duplicates only
sort file.txt | uniq -c    # Count all unique lines
```

### Sed (Stream Editor)
```bash
sed 's/old/new/' file.txt           # Replace first occurrence
sed 's/old/new/g' file.txt          # Replace all occurrences
sed 's/old/new/2' file.txt          # Replace second occurrence
sed '5s/old/new/' file.txt          # Replace on line 5
sed '1,5s/old/new/' file.txt        # Replace lines 1-5
sed '/pattern/s/old/new/' file.txt  # Replace if pattern matches
sed -i 's/old/new/g' file.txt       # In-place edit (modifies file)
sed 'd' file.txt                    # Delete lines
sed '/pattern/d' file.txt           # Delete matching lines
sed 'p' file.txt                    # Print lines (duplicates)
sed -n '5p' file.txt                # Print only line 5
```

### Awk
```bash
awk '{print $1}' file.txt           # Print first field
awk -F: '{print $1}' /etc/passwd    # Print with delimiter
awk '{print $1, $3}' file.txt       # Print fields 1 and 3
awk 'NR==5' file.txt                # Print line 5
awk 'NF>3' file.txt                 # Lines with >3 fields
awk '{sum+=$1} END {print sum}' file # Sum first column
awk '/pattern/ {print}' file.txt    # Print matching lines
```

### Tr (Translate)
```bash
tr 'a-z' 'A-Z' < file.txt  # Convert lowercase to uppercase
tr -d 'characters' file    # Delete characters
tr -s ' ' file.txt         # Squeeze multiple spaces to one
tr '\n' ' ' file.txt       # Replace newlines with spaces
```

---

## 📡 NETWORKING COMMANDS

### Interface Configuration
```bash
ip addr show               # Display all IPs
ip link show               # Network interfaces status
ip route show              # Routing table
ifconfig                   # Network config (legacy)
ifconfig -a                # All interfaces

# Manual configuration
sudo ip addr add 192.168.1.100/24 dev eth0  # Add IP
sudo ip link set eth0 up                    # Enable interface
sudo ip route add default via 192.168.1.1   # Add default route
sudo ip route del 192.168.1.0/24 dev eth0   # Delete route
```

### Connectivity Testing
```bash
ping -c 4 8.8.8.8          # Ping with count
ping 8.8.8.8               # Continuous ping
ping -f 8.8.8.8            # Flood ping (requires root)
ping -i 0.2 8.8.8.8        # Interval between pings
traceroute 8.8.8.8         # Trace path to host
tracepath 8.8.8.8          # Alternative to traceroute
mtr google.com             # Real-time traceroute
```

### DNS Queries
```bash
nslookup google.com        # Simple DNS lookup
nslookup -query=MX domain.com    # MX records
dig google.com             # Detailed DNS info
dig google.com +short      # Short output
dig google.com @8.8.8.8    # Query specific server
host google.com            # Simple hostname lookup
getent hosts google.com    # Query local hosts file
```

### Network Statistics
```bash
netstat -tuln              # Listening ports (legacy)
netstat -an                # All connections
netstat -ap                # Processes with connections
ss -tuln                   # Listening sockets (modern)
ss -tan                    # TCP listening sockets
ss -uan                    # UDP listening sockets
ss -tp                     # Show process names
lsof -i :8080              # What's using port 8080
netstat -i                 # Interface statistics
```

### Connectivity Utilities
```bash
curl http://example.com         # Fetch HTTP content
curl -I http://example.com      # Headers only
curl -X POST http://example.com # POST request
wget http://example.com/file    # Download file
wget -O newname http://...      # Download with new name
ftp hostname                    # FTP connection
sftp user@hostname              # SFTP connection
ssh user@hostname               # SSH connection
ssh -i keyfile user@hostname    # SSH with key
scp file user@host:/path        # Copy to remote
scp user@host:/path/file .      # Copy from remote
rsync -av source/ dest/         # Sync directories
```

---

## 🔐 SSH & SECURITY

### SSH Keys
```bash
ssh-keygen -t rsa -b 4096            # Generate RSA key
ssh-keygen -t ed25519                # Generate ED25519 key (preferred)
ssh-copy-id -i ~/.ssh/id_rsa.pub user@host  # Copy public key
chmod 600 ~/.ssh/id_rsa              # Secure private key
chmod 644 ~/.ssh/id_rsa.pub          # Secure public key
ssh -i keyfile user@hostname         # SSH with specific key
ssh-agent bash                       # Start SSH agent
ssh-add ~/.ssh/id_rsa                # Add key to agent
```

### File Permissions for SSH
```bash
chmod 700 ~/.ssh                     # SSH directory
chmod 644 ~/.ssh/id_rsa.pub         # Public key
chmod 600 ~/.ssh/id_rsa             # Private key (IMPORTANT!)
chmod 644 ~/.ssh/authorized_keys    # Auth keys file
chmod 644 ~/.ssh/config             # SSH config
```

### Secure File Transfer
```bash
scp file user@host:/path             # Copy file to remote
scp user@host:/path/file .           # Copy file from remote
scp -r dir/ user@host:/path/         # Copy directory
sftp user@host                       # Interactive SFTP
```

---

## 🛠️ SYSTEM ADMINISTRATION

### System Information
```bash
uname -a                   # Complete system info
lsb_release -a             # Distribution info
cat /etc/os-release        # OS information
hostname                   # System hostname
uptime                     # System uptime
date                       # Current date/time
timedatectl                # Time and date status (systemd)
who                        # Logged in users
w                          # Users with details
last                       # Login history
```

### Disk & Storage
```bash
df -h                      # Disk space usage
df -i                      # Inode usage
du -sh .                   # Current directory size
du -sh /*                  # Size of all directories
lsblk                      # Block devices
fdisk -l                   # Disk partitions (root)
mount                      # Mounted filesystems
mount /dev/sda1 /mnt       # Mount partition
umount /mnt                # Unmount partition
```

### Process Management
```bash
ps aux                     # All processes
ps aux | grep process      # Find process
ps -ef                     # Full process listing
ps -eo pid,user,cmd        # Custom columns
top                        # Real-time processes
htop                       # Enhanced process viewer
pgrep processname          # Find PID by name
pkill processname          # Kill by name
kill PID                   # Graceful kill
kill -9 PID                # Force kill
killall processname        # Kill all instances
```

### Process Priority
```bash
nice -n 10 command         # Run with low priority
renice -n 5 -p PID         # Adjust running process
ps -o pid,nice,cmd         # Show priorities
```

### Memory & CPU
```bash
free -h                    # Memory usage
free -m                    # Memory in MB
vmstat 1 5                 # Memory stats every 1s, 5 times
mpstat                     # CPU statistics
iostat                     # I/O statistics
lscpu                      # CPU information
```

---

## 👨‍💻 COMMAND HISTORY & ALIASES

### History
```bash
history                    # Show command history
history 20                 # Last 20 commands
!!                         # Repeat last command
!command                   # Repeat last command starting with
!$                         # Last argument of previous command
!*                         # All arguments of previous command
history -c                 # Clear history
history -d NUM             # Delete specific line
HISTSIZE=1000              # Set history size (in .bashrc)
```

### Aliases
```bash
alias ll='ls -la'          # Create alias
alias                      # List all aliases
unalias ll                 # Remove alias
alias -p                   # Print all aliases

# Common useful aliases
alias cd..='cd ..'
alias grep='grep --color=auto'
alias ls='ls -la'
alias la='ls -la'
alias ll='ls -l'
alias mkdir='mkdir -p'
alias cp='cp -i'           # Confirm before overwrite
alias mv='mv -i'
alias rm='rm -i'
```

---

## 🔥 FIREWALL & SECURITY

### Sudo
```bash
sudo command               # Execute as root
sudo -i                    # Root shell
sudo -u user command       # Execute as specific user
sudo -l                    # List user's privileges
sudo visudo                # Edit sudoers safely

# Sudoers file examples
ALL=(ALL) ALL              # Full access
ALL=(ALL) NOPASSWD:ALL     # No password required
ALL=(root) /usr/bin/systemctl  # Specific command only
```

### Firewall (UFW)
```bash
sudo ufw enable            # Enable firewall
sudo ufw disable           # Disable firewall
sudo ufw status            # Check status
sudo ufw allow 22/tcp      # Allow SSH
sudo ufw allow 80/tcp      # Allow HTTP
sudo ufw allow 443/tcp     # Allow HTTPS
sudo ufw deny 23/tcp       # Deny Telnet
sudo ufw delete allow 22   # Remove rule
sudo ufw reset             # Reset all rules
```

### Firewall (iptables)
```bash
sudo iptables -L           # List rules
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT  # Allow SSH
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT  # Allow HTTP
sudo iptables -D INPUT -p tcp --dport 80 -j ACCEPT  # Delete rule
sudo iptables -F           # Flush all rules
```

---

## 📝 FILE COMPRESSION

### Tar
```bash
tar -cf archive.tar file1 file2        # Create tar
tar -czf archive.tar.gz files/         # Create gzipped
tar -cjf archive.tar.bz2 files/        # Create bzipped
tar -xf archive.tar                    # Extract tar
tar -xzf archive.tar.gz                # Extract gzipped
tar -xjf archive.tar.bz2               # Extract bzipped
tar -tzf archive.tar.gz                # List contents
tar -tvf archive.tar                   # Verbose list
```

### Zip
```bash
zip archive.zip file1 file2            # Create zip
zip -r archive.zip directory/          # Zip directory
unzip archive.zip                      # Extract
unzip -l archive.zip                   # List contents
zip -d archive.zip file                # Delete file from zip
```

### Compression Tools
```bash
gzip file                  # Compress (file.gz)
gunzip file.gz             # Decompress
bzip2 file                 # Compress (file.bz2)
bunzip2 file.bz2           # Decompress
xz file                    # Compress (file.xz)
unxz file.xz               # Decompress
```

---

## 🖥️ USEFUL ONE-LINERS

```bash
# Find and delete old files
find / -type f -mtime +30 -delete

# Find large files
find / -type f -size +100M -exec ls -lh {} \;

# List open files
lsof

# Monitor file changes in real-time
watch -n 1 'ps aux | grep process'

# Extract IP addresses from file
grep -oE '\b([0-9]{1,3}\.){3}[0-9]{1,3}\b' file

# Count lines in multiple files
wc -l *.txt

# Compare files
diff file1 file2

# Show differences with side-by-side
diff -y file1 file2

# Find and replace recursively
find . -type f -name "*.txt" -exec sed -i 's/old/new/g' {} \;

# List processes using most memory
ps aux | sort -nk 4 | tail -5

# List processes using most CPU
ps aux | sort -nk 3 | tail -5

# Find duplicate files
find . -type f -exec md5sum {} \; | sort | uniq -d -w32

# Recursive grep with file count
grep -r "pattern" . | wc -l

# Download multiple files
wget -i urls.txt

# Create directory structure from file listing
cat filelist.txt | xargs -I {} mkdir -p $(dirname {})

# Kill all processes matching pattern
killall -9 process_name

# Monitor network connections
watch -n 1 'ss -tan'

# Continuous file monitoring
watch -n 0.1 'ls -la'

# Execute command at specific time
echo "command" | at 14:30

# Schedule recurring task
(crontab -l; echo "0 2 * * * /path/to/script.sh") | crontab -

# Generate random password
openssl rand -base64 12

# Check port availability
netstat -tuln | grep :8080

# Get external IP
curl ifconfig.me

# Find what's using bandwidth
nethogs

# SSH without checking known_hosts
ssh -o StrictHostKeyChecking=no user@host

# Create reverse tunnel
ssh -R 3000:localhost:3000 user@host

# Create SSH jump/bastion
ssh -J user@bastion user@target

# Tar with progress
pv large_file | tar czf - | pv > archive.tar.gz

# Monitor system in real-time
dstat

# Show network connections with process info
ss -tp

# Count unique values in column
cut -d: -f1 /etc/passwd | sort | uniq | wc -l
```

---

## ⚠️ DANGEROUS COMMANDS (USE WITH CAUTION)

```bash
rm -rf /                   # DELETE ENTIRE SYSTEM!
dd if=/dev/zero of=/dev/sda  # WIPE ENTIRE DISK!
:(){ :|:& };:             # FORK BOMB - Crashes system
:>file                     # Truncate file (ok) but used in aliases
cat /dev/zero > file       # Fill disk space quickly
```

---

## 📚 HELP & DOCUMENTATION

```bash
man command                # Manual page
man -k keyword             # Search manuals
help command               # Built-in help
command --help             # Quick help
command -h                 # Short help
whatis command             # One-line description
info command               # Info documentation
```

---

**Remember:** With great power comes great responsibility!  
Always be careful with privileged commands, especially those involving deletion.

---

**Last Updated:** March 2025
**For:** ApexPlanet Cybersecurity & Ethical Hacking Internship
