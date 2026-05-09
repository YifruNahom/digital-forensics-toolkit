# Digital Forensics Quick Reference Cheat Sheet

## 🚀 Quick Commands

### Evidence Acquisition

```bash
# Image entire disk (Linux)
sudo dd if=/dev/sda of=/evidence/disk.img bs=4M status=progress conv=noerror,sync

# Memory capture (Windows)
DumpIt.exe

# Hash a file
md5sum file.bin
sha256sum file.bin
certutil -hashfile file.exe SHA256  # Windows
```

---

### Volatility (Memory Forensics)

```bash
# Identify profile
volatility -f mem.dmp imageinfo

# Process list
volatility -f mem.dmp --profile=Win10x64 pslist

# Hidden processes
volatility -f mem.dmp --profile=Win10x64 psscan

# Network connections
volatility -f mem.dmp --profile=Win10x64 netscan

# DLL list for process
volatility -f mem.dmp --profile=Win10x64 dlllist -p 1234

# Registry hives
volatility -f mem.dmp --profile=Win10x64 hivelist

# Dump process
volatility -f mem.dmp --profile=Win10x64 procdump -p 1234 -D output/

# Command history
volatility -f mem.dmp --profile=Win10x64 cmdscan

# Malware detection
volatility -f mem.dmp --profile=Win10x64 malfind
```

---

### Wireshark Display Filters

```
# IP filtering
ip.addr == 192.168.1.10
ip.src == 192.168.1.10

# Port filtering
tcp.port == 80
tcp.port == 443

# Protocol filtering
http
ftp
dns
smtp
ssh

# TCP flags
tcp.flags.syn == 1
tcp.flags.push == 1

# Search for keywords
frame contains "password"
http.request.uri contains "login"

# Follow stream
Right-click → Follow → TCP Stream
```

---

### Windows Registry (Forensics)

```powershell
# View Run keys
Get-ItemProperty "HKLM:\Software\Microsoft\Windows\CurrentVersion\Run"
Get-ItemProperty "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run"

# Recently opened files
HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs

# USB devices
HKLM\SYSTEM\CurrentControlSet\Enum\USBSTOR

# Network interfaces
HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces

# Timezone
HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation
```

---

### PowerShell Forensics

```powershell
# Process list
Get-Process | Export-Csv processes.csv -NoTypeInformation

# Services
Get-Service | Where-Object {$_.Status -eq "Running"}

# Network connections
Get-NetTCPConnection | Select LocalAddress,LocalPort,RemoteAddress,RemotePort,State

# File hash
Get-FileHash C:\suspect.exe -Algorithm SHA256

# Event logs
Get-EventLog -LogName Security -Newest 100
Get-EventLog -LogName System -Newest 100

# Startup programs
Get-CimInstance Win32_StartupCommand | Select-Object Name, Command, Location

# Scheduled tasks
Get-ScheduledTask | Where-Object {$_.State -ne "Disabled"}
```

---

### Linux Forensics

```bash
# List processes
ps aux

# Network connections
netstat -tulpn
ss -tulpn

# Active users
who
w
last

# File access times
stat filename

# Find recently modified files
find / -mtime -1 2>/dev/null

# Search for files
find / -name "*.log" 2>/dev/null

# View logs
tail -f /var/log/syslog
journalctl -xe

# Installed packages (Debian/Ubuntu)
dpkg -l

# Installed packages (RHEL/CentOS)
rpm -qa

# Cron jobs
crontab -l
cat /etc/crontab
ls -la /etc/cron.*
```

---

### File Recovery

```bash
# PhotoRec (file carving)
sudo photorec /dev/sdb

# TestDisk (partition recovery)
sudo testdisk /dev/sdb

# Extract RAR archive
rar e archive.rar

# Extract ZIP
unzip archive.zip
```

---

## 🔑 Key File Locations

### Windows Artifacts

```
# User profiles
C:\Users\[username]\NTUSER.DAT
C:\Users\[username]\AppData\

# Registry hives
C:\Windows\System32\config\SAM
C:\Windows\System32\config\SECURITY
C:\Windows\System32\config\SYSTEM
C:\Windows\System32\config\SOFTWARE

# Event logs
C:\Windows\System32\winevt\Logs\

# Prefetch
C:\Windows\Prefetch\

# Recent files
C:\Users\[username]\AppData\Roaming\Microsoft\Windows\Recent\

# Browser history (Chrome)
C:\Users\[username]\AppData\Local\Google\Chrome\User Data\Default\History

# Temp files
C:\Users\[username]\AppData\Local\Temp\
C:\Windows\Temp\

# Recycle Bin
C:\$Recycle.Bin\
```

---

### Linux Artifacts

```
# User home directories
/home/[username]/

# System logs
/var/log/syslog
/var/log/auth.log
/var/log/messages

# User login history
/var/log/wtmp
/var/log/btmp

# Bash history
~/.bash_history

# Cron jobs
/etc/crontab
/var/spool/cron/crontabs/

# SSH keys
~/.ssh/

# Web server logs
/var/log/apache2/
/var/log/nginx/
```

---

### Mobile (Android)

```
# Contacts
/data/data/com.android.providers.contacts/databases/contacts2.db

# SMS/MMS
/data/data/com.android.providers.telephony/databases/mmssms.db

# Call logs
/data/data/com.android.providers.contacts/databases/contacts2.db

# Browser history
/data/data/com.android.browser/databases/browser.db

# Application data
/data/data/[package_name]/
```

---

### Mobile (iOS)

```
# Contacts
Library/AddressBook/AddressBook.sqlitedb

# Messages
Library/SMS/sms.db

# Call history
Library/CallHistory/call_history.db

# Calendar
Library/Calendar/Calendar.sqlitedb

# Safari history
Library/Safari/History.db

# Notes
Library/Notes/notes.sqlite
```

---

## 🎯 Incident Response Checklist

### Initial Response
- [ ] Document current date/time
- [ ] Photograph screen if visible
- [ ] Note running processes/applications
- [ ] Capture volatile data (memory first!)
- [ ] Document network connections
- [ ] Preserve logs

### Evidence Collection
- [ ] Memory dump
- [ ] Disk image (with write-blocker)
- [ ] Network packet capture
- [ ] Log files
- [ ] Running process list
- [ ] Open ports/connections

### Analysis Phase
- [ ] Timeline reconstruction
- [ ] Malware identification
- [ ] Data exfiltration check
- [ ] Persistence mechanisms
- [ ] Lateral movement
- [ ] Initial access vector

### Documentation
- [ ] Chain of custody forms
- [ ] Timeline of events
- [ ] Tools used (with versions)
- [ ] Hash values of evidence
- [ ] Findings report
- [ ] Recommendations

---

## 🚨 Red Flags

### Processes
- Processes running from temp directories
- Misspelled system process names (svch0st.exe)
- High CPU/network from unexpected processes
- Processes without parent processes
- Hidden processes

### Network
- Outbound connections on unusual ports
- Connections to foreign IPs
- High volumes of outbound traffic
- DNS queries to suspicious domains
- Beaconing behavior (regular intervals)

### Files
- Files in unusual locations
- Recently modified system files
- Hidden files in system directories
- High-entropy executables (>7.0)
- Files with mismatched extensions

### Registry
- New Run key entries
- Modified startup locations
- Unknown scheduled tasks
- New services
- Disabled security software

---

## 📊 Hash Databases

### NSRL (National Software Reference Library)
- Known good software hashes
- Helps filter out benign files
- https://www.nist.gov/itl/ssd/software-quality-group/national-software-reference-library-nsrl

### VirusTotal
- Malware hash database
- Multi-engine scanning
- https://www.virustotal.com

---

## 🔗 Useful Resources

- **SANS Posters:** https://www.sans.org/posters/
- **Volatility Wiki:** https://github.com/volatilityfoundation/volatility/wiki
- **Wireshark Docs:** https://www.wireshark.org/docs/
- **Autopsy Docs:** https://sleuthkit.org/autopsy/docs/
- **NIST Computer Forensics:** https://www.nist.gov/forensics

---

## 📞 Emergency Contacts

### Reporting
- **FBI IC3:** https://www.ic3.gov
- **CISA:** https://www.cisa.gov/report
- **Local Law Enforcement**

### Tools Support
- **Autopsy Forums:** https://sleuthkit.discourse.group/
- **Volatility Community:** https://github.com/volatilityfoundation

---

**Last Updated:** May 2026  
**Created by:** Nahom Yifru
