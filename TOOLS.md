# Digital Forensics Tools Reference

## 📑 Table of Contents
- [Evidence Acquisition](#evidence-acquisition)
- [Analysis Platforms](#analysis-platforms)
- [Memory Forensics](#memory-forensics)
- [Network Analysis](#network-analysis)
- [Mobile Forensics](#mobile-forensics)
- [File Recovery](#file-recovery)
- [Hashing & Verification](#hashing--verification)
- [Utilities](#utilities)

---

## 🔧 Evidence Acquisition

### FTK Imager
**Purpose:** Forensic disk imaging and file extraction  
**Developer:** AccessData (Exterro)  
**Platform:** Windows  
**License:** Free

**Key Features:**
- Create forensic images (E01, DD, AFF)
- Preview file systems without mounting
- Extract files and folders
- Generate hash values (MD5, SHA-1)
- Memory capture
- Mount images as read-only

**Common Usage:**
```
1. Add Evidence Item → Physical Drive / Logical Drive / Image File
2. Navigate file system in Evidence Tree
3. Right-click files → Export Files
4. File → Capture Memory (for RAM dumps)
5. Verify Image → Check hash values
```

**Output Formats:**
- E01 (EnCase)
- SMART
- AFF (Advanced Forensic Format)
- Raw (DD)

---

### DumpIt
**Purpose:** Windows memory acquisition  
**Developer:** Comae Technologies  
**Platform:** Windows  
**License:** Free

**Usage:**
```cmd
DumpIt.exe /O C:\Evidence\memory.dmp
```

**Features:**
- Fast memory acquisition
- No installation required
- Minimal user interaction
- Automatic output naming

---

### dd (Linux)
**Purpose:** Disk imaging and data conversion  
**Platform:** Linux/Unix  
**License:** Open Source (GPL)

**Common Commands:**
```bash
# Create disk image
sudo dd if=/dev/sda of=/mnt/evidence/disk.img bs=4M status=progress

# Create USB image
sudo dd if=/dev/sdb of=/evidence/usb.dd bs=1M

# Wipe drive (zeros)
sudo dd if=/dev/zero of=/dev/sdb bs=1M

# Wipe drive (random)
sudo dd if=/dev/urandom of=/dev/sdb bs=1k seek=200 count=4k
```

---

## 🔍 Analysis Platforms

### Paraben's E3 (Electronic Evidence Examiner)
**Purpose:** Comprehensive digital forensics platform  
**Developer:** Paraben Corporation  
**Platform:** Windows  
**License:** Commercial

**Capabilities:**
- **File Systems:** NTFS, FAT, Ext2/3/4, HFS+, APFS
- **Deleted File Recovery**
- **Registry Analysis**
- **Email Examination**
- **Mobile Device Analysis** (iOS, Android)
- **Hash Calculation & Verification**

**Key Modules:**
- Case Management
- Sorted Files (file type categorization)
- Registry Analysis
- Email Analysis
- Internet History
- Process Analysis (from memory dumps)

**Workflow:**
```
1. Create New Case
2. Add Evidence → Select image/device
3. Process Evidence → Choose analysis modules
4. Analyze → Navigate evidence tree
5. Tag relevant items
6. Generate Report
```

---

### Autopsy
**Purpose:** Open-source digital forensics platform  
**Developer:** Sleuth Kit / Basis Technology  
**Platform:** Windows, Linux, macOS  
**License:** Open Source (Apache 2.0)

**Features:**
- **Timeline Analysis**
- **Keyword Search**
- **File Type Detection**
- **EXIF Extraction**
- **Registry Analysis (RegRipper)**
- **Web Artifacts**
- **Android/iOS Support**

**Ingest Modules:**
- Recent Activity
- Hash Lookup
- File Type Identification
- Keyword Search
- Email Parser
- Encryption Detection
- Interesting Files Identifier

**Usage Flow:**
```
1. Create New Case
2. Add Data Source → Disk Image / Local Disk
3. Configure Ingest Modules
4. Wait for processing
5. Review Results → Tree view / Timeline / Keyword Hits
6. Export artifacts
```

---

## 🧠 Memory Forensics

### Volatility
**Purpose:** Advanced memory forensics framework  
**Developer:** Volatility Foundation  
**Platform:** Windows, Linux, macOS  
**License:** Open Source (GPL)

**Common Commands:**
```bash
# Identify memory profile
volatility -f memory.dmp imageinfo

# List processes
volatility -f memory.dmp --profile=Win10x64 pslist

# Scan for hidden processes
volatility -f memory.dmp --profile=Win10x64 psscan

# Network connections
volatility -f memory.dmp --profile=Win10x64 netscan

# List DLLs for a process
volatility -f memory.dmp --profile=Win10x64 dlllist -p 1234

# Dump process memory
volatility -f memory.dmp --profile=Win10x64 memdump -p 1234 -D output/

# Registry hives
volatility -f memory.dmp --profile=Win10x64 hivelist

# Command history
volatility -f memory.dmp --profile=Win10x64 cmdscan
```

**Key Plugins:**
- `pslist` - Process listing
- `psscan` - Scan for processes (finds hidden)
- `netscan` - Network connections
- `malfind` - Malware detection
- `yarascan` - YARA rule scanning
- `handles` - Open handles
- `filescan` - File objects in memory

---

### DensityScout
**Purpose:** Entropy-based malware detection  
**Developer:** Christian Wojner  
**Platform:** Windows  
**License:** Free

**Usage:**
```cmd
densityscout.exe -p C:\ExtractedFiles\
```

**Purpose:** Detects packed/encrypted executables by analyzing entropy (randomness) of data.

---

## 🌐 Network Analysis

### Wireshark
**Purpose:** Network protocol analyzer  
**Developer:** Wireshark Foundation  
**Platform:** Windows, Linux, macOS  
**License:** Open Source (GPL)

**Key Features:**
- **Live packet capture**
- **Offline PCAP analysis**
- **Protocol dissection** (1000+ protocols)
- **Display filters**
- **Stream reconstruction**
- **Statistics & graphs**

**Essential Display Filters:**
```
# IP-based filtering
ip.addr == 192.168.1.10
ip.src == 192.168.1.10 && ip.dst == 172.16.0.2

# Port-based filtering
tcp.port == 80
tcp.srcport == 443

# Protocol filtering
http
ftp
dns
smtp

# Flag filtering
tcp.flags.push == 1
tcp.flags.syn == 1 && tcp.flags.ack == 0

# Keyword search
frame contains "password"
http.request.uri contains "admin"
```

**Follow Streams:**
```
Right-click packet → Follow → TCP Stream
Right-click packet → Follow → HTTP Stream
```

**Export Objects:**
```
File → Export Objects → HTTP / FTP / SMB
```

**Common Tasks:**
- **FTP Credential Capture:** Filter `ftp`, look for USER/PASS commands
- **HTTP Reconstruction:** Follow TCP Stream, save as raw
- **File Transfer Analysis:** Export Objects → HTTP
- **Malware C2 Detection:** Look for unusual port traffic

---

## 📱 Mobile Forensics

### iOS Backup Analysis (Paraben's E3)
**Supported Data:**
- Contacts
- Messages (iMessage, SMS)
- Call History
- Calendar events
- Notes
- Photos
- Safari History
- Application Data

**Key Databases:**
- `AddressBook.sqlitedb` - Contacts
- `sms.db` - Messages
- `Calendar.sqlitedb` - Calendar events
- `Notes.sqlite` - Notes
- `History.db` - Safari browsing history

---

### Android Forensics
**Supported Data:**
- Contacts (`contacts2.db`)
- SMS/MMS (`mmssms.db`)
- Call Logs (`contacts2.db`)
- Application Data
- Installed Apps
- User Activity Timeline

**Key File Paths:**
```
/data/data/com.android.providers.contacts/databases/contacts2.db
/data/data/com.android.providers.telephony/databases/mmssms.db
/data/system/users/0/accounts.db
/data/data/<app_package>/databases/
```

---

## 💾 File Recovery

### PhotoRec
**Purpose:** File carving and recovery  
**Developer:** CGSecurity  
**Platform:** Windows, Linux, macOS  
**License:** Open Source (GPL)

**Usage:**
```bash
sudo photorec /dev/sdb
```

**Features:**
- Recovers 300+ file types
- Works on damaged filesystems
- File signature-based recovery
- Cross-platform

**Supported Types:**
- Images: JPG, PNG, GIF, BMP
- Documents: PDF, DOC, XLS, PPT
- Archives: ZIP, RAR, 7Z, TAR
- Multimedia: MP3, MP4, AVI, MOV

---

## 🔐 Hashing & Verification

### MD5 / SHA-1 / SHA-256
**Purpose:** File integrity verification

**Windows (CertUtil):**
```cmd
certutil -hashfile file.exe MD5
certutil -hashfile file.exe SHA1
certutil -hashfile file.exe SHA256
```

**Linux:**
```bash
md5sum file.bin
sha1sum file.bin
sha256sum file.bin
```

**PowerShell:**
```powershell
Get-FileHash file.exe -Algorithm MD5
Get-FileHash file.exe -Algorithm SHA1
Get-FileHash file.exe -Algorithm SHA256
```

---

## 🔧 Utilities

### RAR
**Purpose:** Archive extraction

```bash
# Extract RAR archive
rar e archive.rar

# List contents
rar l archive.rar
```

---

### PowerShell (Forensics)
**Key Commands:**
```powershell
# Get process list
Get-Process

# List services
Get-Service

# View registry key
Get-ItemProperty "HKLM:\Software\Microsoft\Windows\CurrentVersion\Run"

# Network connections
Get-NetTCPConnection

# Event logs
Get-EventLog -LogName Security -Newest 100

# File hash
Get-FileHash C:\Evidence\file.exe -Algorithm SHA256

# Export to CSV
Get-Process | Export-Csv processes.csv -NoTypeInformation
```

---

## 📊 Comparison Matrix

| Tool | Evidence Acquisition | Analysis | Memory | Network | Mobile |
|------|---------------------|----------|--------|---------|--------|
| FTK Imager | ✅ | ⚠️ | ✅ | ❌ | ❌ |
| E3 | ⚠️ | ✅ | ✅ | ❌ | ✅ |
| Autopsy | ⚠️ | ✅ | ⚠️ | ❌ | ✅ |
| Volatility | ❌ | ✅ | ✅ | ❌ | ❌ |
| Wireshark | ❌ | ✅ | ❌ | ✅ | ❌ |
| PhotoRec | ❌ | ✅ | ❌ | ❌ | ❌ |

✅ Fully Supported | ⚠️ Limited Support | ❌ Not Supported

---

## 📚 Additional Resources

- [Sleuth Kit Documentation](https://www.sleuthkit.org/sleuthkit/)
- [Volatility Command Reference](https://github.com/volatilityfoundation/volatility/wiki/Command-Reference)
- [Wireshark Display Filter Reference](https://www.wireshark.org/docs/dfref/)
- [SANS Digital Forensics Posters](https://www.sans.org/posters/)

---

**Last Updated:** May 2026  
**Maintained by:** Nahom Yifru
