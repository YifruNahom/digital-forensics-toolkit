# Lab 02: Recovering Deleted and Damaged Files

## 🎯 Objective
Master techniques for recovering deleted files from various file systems (NTFS, Ext4) using multiple forensic tools.

## 📚 Topics Covered
- Deleted file recovery
- File system analysis (NTFS, Ext4)
- File carving techniques
- Archive extraction
- Cross-tool verification

## 🛠 Tools Used
- **Paraben's E3** - NTFS file recovery
- **Autopsy** - Ext4 file recovery
- **PhotoRec** - Linux file recovery
- **RAR** - Archive extraction

## 📝 Lab Procedures

### Part 1: NTFS File Recovery with E3

**Scenario:** Recover deleted files from NTFS drive image

**Steps:**
1. Launch E3 and load NTFS drive image
2. Navigate to Trash folder
3. View list of recovered files:
   - `NTFS.dbx` (Email database)
   - `BTCWallet` files
   - Patent documents (`10791442.pdf`)
   - `IMG_20201222.jpg`
   - Various `.txt` files

4. Extract recovered files
5. Verify with File Viewer

**Files Recovered:**
- `10791442.pdf` - US Patent document
- `BTCWallet.exe` - Bitcoin wallet application
- `NTFS.dbx` - Email messages
- `IMG_20201222.jpg` - Image file
- Multiple text documents

### Part 2: Ext4 File Recovery with Autopsy

**Scenario:** Analyze Ext4 Linux filesystem for deleted files

**Steps:**
1. Create new case in Autopsy
2. Add Ext4 drive image as data source
3. Navigate to "Deleted Files" view
4. Identify target files:
   - `buff.ataldatabase.dmroot4.crapted.tpg`
   - `xiao.Keyword/gnublic.crapted.tpg`
   - `USI01742.pdf` (Patent file)

5. Extract deleted files
6. View recovered patent document

**Recovered Evidence:**
```
File: USI01742.pdf
Type: PDF Document
Status: Deleted (Recoverable)
Patent No.: US 10,791,442 B2
Date: Sep. 29, 2020
Title: System and Method for Remote Asset Management
```

### Part 3: Linux File Recovery with PhotoRec

**Scenario:** Recover files from damaged Linux partition

**Commands Used:**
```bash
# List available drives
sudo fdisk -l

# Attempt to mount (fails due to corruption)
sudo mount /dev/sdb2 /mnt/media

# Use dd to create image
sudo dd if=/dev/urandom of=/dev/sdb2 bs=1k seek=200 count=4k

# Run PhotoRec
sudo photorec

# Install testdisk if needed
sudo apt-get install testdisk

# Extract from RAR archive
cd documents/recup_dir.1
sudo rar e f8444432.rar
```

**Recovered Files:**
- `.recoveryKeys.txt.swp`
- `.recoveryKeys.txt.swo`
- `serverImage.dd`
- `recoveryKeys.txt`

**RAR Archive Contents:**
```
Extracting from f8444432.rar

Extracting .recoveryKeys.txt.swp    OK
Extracting .recoveryKeys.txt.swo    OK  
Extracting serverImage.dd           OK
Extracting recoveryKeys.txt         OK
All OK
```

## 🔑 Key Concepts

### File System Structures

**NTFS (Windows)**
- Master File Table (MFT) tracks all files
- Deleted files remain until overwritten
- $Recycle.Bin stores deleted files
- File metadata preserved in MFT

**Ext4 (Linux)**
- Inode table contains file metadata
- Directory entries removed on deletion
- Data blocks remain until reallocated
- Journal may contain recovery information

### File Recovery Process

1. **Identify file system type**
2. **Scan for deleted file entries**
3. **Reconstruct file metadata**
4. **Extract file contents**
5. **Verify file integrity**

### File Carving
Technique to recover files without file system metadata by:
- Scanning for file signatures (headers)
- Identifying file type patterns
- Extracting data between signatures
- Reconstructing files

## 📊 Results Summary

### E3 (NTFS Recovery)
- **Files Scanned:** 56
- **Files Recovered:** 23
- **Success Rate:** ~41%
- **Key Evidence:** Patent PDF, Email database, Bitcoin wallet

### Autopsy (Ext4 Recovery)
- **Deleted Files Found:** 8
- **Successfully Recovered:** 6
- **Patent Document:** ✅ Recovered
- **Encrypted Files:** Identified but not decrypted

### PhotoRec (File Carving)
- **Compressed Files:** 1 RAR archive recovered
- **Archive Contents:** 4 files extracted
- **Recovery Keys:** ✅ Found
- **Server Image:** ✅ Retrieved

## 💡 Best Practices

### Before Recovery
✅ Image the drive first (never work on original)  
✅ Use write-blockers  
✅ Document initial state  
✅ Generate hash of original  

### During Recovery
✅ Use multiple tools (increases success rate)  
✅ Export recovered files to separate location  
✅ Generate hashes of recovered files  
✅ Document recovery process  

### After Recovery
✅ Verify file integrity  
✅ Check file contents  
✅ Compare with known good hashes  
✅ Document findings  

## ⚠️ Common Pitfalls

❌ **Writing to evidence drive** - Always use read-only access  
❌ **Insufficient space** - Recovery destination needs adequate space  
❌ **Overlooking fragments** - Partial files may still contain evidence  
❌ **Ignoring metadata** - Timestamps and attributes are valuable  
❌ **Single tool reliance** - Different tools find different files  

## 🎓 Skills Demonstrated

- ✅ NTFS file system analysis
- ✅ Ext4 file system analysis
- ✅ Deleted file recovery (multiple tools)
- ✅ File carving techniques
- ✅ Archive extraction
- ✅ Linux command-line forensics
- ✅ Cross-tool verification

## 🔬 Technical Details

### NTFS Recovery Process
```
1. Parse MFT (Master File Table)
2. Identify deleted entries ($FILE_NAME marked as deleted)
3. Check if data clusters are allocated
4. Extract data from unallocated clusters
5. Reconstruct file
```

### Ext4 Recovery Challenges
```
- Directory entries removed on delete
- Inode metadata often zeroed
- Must rely on file carving
- Journal may help recovery
- Extents complicate recovery
```

### PhotoRec File Types Recovered
- **Archives:** RAR, ZIP, 7Z
- **Images:** JPG, PNG, GIF
- **Documents:** PDF, DOC, TXT
- **Databases:** SQLite, DBX
- **Executables:** EXE, DLL

## 📖 References

- NTFS File System Specification
- Ext4 Filesystem Documentation
- PhotoRec User Guide
- Autopsy User Documentation

## 🔗 Related Labs

- [Lab 01: Chain of Custody](../01-chain-of-custody/)
- [Lab 04: Windows Forensics](../04-windows-forensics/)

---

**Time on Task:** 2 hours, 39 minutes  
**Completion:** 81%  
**Date:** February 25, 2026
