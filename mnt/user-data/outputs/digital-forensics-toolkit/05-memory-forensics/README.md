# Lab 05: Memory Forensics & Malware Detection

## 🎯 Objective
Analyze volatile system memory to detect malware, identify malicious processes, and uncover network connections using memory forensics tools.

## 📚 Topics Covered
- Memory acquisition (RAM dumps)
- Process analysis
- Malware detection (keyloggers, trojans)
- Hidden process identification
- Network connection enumeration
- Registry key analysis from memory

## 🛠 Tools Used
- **DumpIt** - Memory acquisition
- **FTK Imager** - Memory capture
- **Paraben's E3** - Process analysis
- **Volatility** - Advanced memory forensics
- **DensityScout** - Entropy analysis

## 📝 Lab Procedures

### Part 1: Memory Capture with DumpIt

**Steps:**
1. Run `DumpIt.exe` as administrator
2. Accept memory capture
3. Wait for completion notification
4. Verify `.dmp` file created

**Command:**
```cmd
DumpIt.exe
```

**Output:**
```
Address space size: 5369700224 bytes (~5120 MB)
Output: C:\WorkStation-20260422-164243.dmp

Successfully captured!
```

**Success Criteria:**
✅ Memory dump file created  
✅ File size matches RAM size  
✅ No errors during capture  

### Part 2: Memory Capture with FTK Imager

**Steps:**
1. Launch FTK Imager
2. File → Capture Memory
3. Specify destination path
4. Wait for capture completion
5. Verify `.mem` file

**Advantages:**
- Creates AD1 (AccessData) format
- Includes system information
- Better compatibility with some tools

### Part 3: Process Analysis with E3

**Analyzed Processes:**

| Process Name | PID | Start Time | Suspicious? |
|--------------|-----|------------|-------------|
| System | 4 | 7/12/21 4:24:52 | ❌ No |
| explorer.exe | 488 | 7/12/21 4:24:54 | ❌ No |
| conhost.exe | 504 | 7/12/21 varies | ⚠️ Multiple instances |
| **hooker.exe** | 620 | 7/12/21 6:42:43 | ✅ **MALICIOUS** |
| svchost.exe | Multiple | varies | ⚠️ Verify each |

**Key Findings:**

#### 1. conhost.exe Analysis
**What is it?**  
Console Window Host - legitimate Windows process that manages command-line applications.

**Normal Behavior:**
- Parent process: `csrss.exe`
- Runs when CMD/PowerShell launched
- Multiple instances normal

**Red Flags:**
- ❌ Running without parent process
- ❌ Located outside `C:\Windows\System32`
- ❌ High CPU with no console windows

**Verdict:** ✅ Legitimate in this case

---

#### 2. hooker.exe Analysis 🚨

**What is it?**  
Password-stealing Trojan and keylogger

**Purpose:**
- Records keyboard inputs
- Monitors applications
- Steals sensitive data
- Exfiltrates credentials

**Evidence Found:**

**Registry Keys Opened:**
```
HKEY_CURRENT_USER\Software\Classes\CPRWorkInstExportXMLs
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\TypedURLs
```

**Files Opened:**
```
C:\Users\Beverly Gates\AppData\Local\Microsoft\Windows\UsrClass.dat
C:\Windows\Fonts\cour.ttf
C:\Windows\System32\en-US\mswsock.dll.mui
C:\Users\Beverly Gates\NTUSER.DAT
```

**Network Activity:** Port 56610 (TCP)

**Indicators of Compromise (IOCs):**
- ✅ Keystroke capture behavior
- ✅ Registry persistence mechanisms
- ✅ File system monitoring
- ✅ Network beaconing
- ✅ Unusual parent process

**Verdict:** 🚨 **CONFIRMED MALWARE**

---

### Part 4: Memory Analysis with Volatility

**Profile Identification:**
```bash
volatility -f memory.dmp imageinfo
```

**Process Listing:**
```bash
volatility -f memory.dmp --profile=Win10x64 pslist
```

**Hidden Process Scan:**
```bash
volatility -f memory.dmp --profile=Win10x64 psscan
```

**Findings:**
```
Hidden Processes Detected:
- conhost.exe (multiple instances flagged)
- hooker.exe (hidden from standard process list)
```

**Network Connections:**
```bash
volatility -f memory.dmp --profile=Win10x64 netscan
```

**Results:**
```
Offset     Proto  Local Address          Foreign Address        State    PID
--------------------------------------------------------------------------
0x1a2b3c4d TCP    192.168.1.10:56610    205.134.253.10:4444   ESTABLISHED  620
```

**🚨 C2 Communication Detected:**
- **Local:** 192.168.1.10:56610
- **Remote:** 205.134.253.10:4444 (Command & Control server)
- **Process:** hooker.exe (PID 620)
- **Protocol:** TCP

**Port 56610 Analysis:**
Categorized as dynamic/private port, used for:
- Apple Xsan Filesystem Access (legitimate)
- **OR** Malware C2 communication (in this case)

---

### Part 5: Entropy Analysis with DensityScout

**Purpose:** Detect packed or encrypted malware executables

**Command:**
```cmd
densityscout.exe -p C:\ExtractedFiles\
```

**How it works:**
- Calculates Shannon entropy
- High entropy (>7.0) = likely packed/encrypted
- Normal executables: 4.5-6.5
- Packed malware: 7.0-8.0

**Results:**
```
File: hooker.exe
Entropy: 7.4
Verdict: SUSPICIOUS (likely packed)
```

---

## 🔑 Key Concepts

### Memory Forensics Advantages

**Why analyze memory?**
1. **Volatile Evidence** - Data that exists only while system is running
2. **Malware Detection** - Rootkits visible in RAM but not on disk
3. **Network Connections** - Active connections only exist in memory
4. **Decrypted Data** - Encryption keys, plaintext passwords
5. **Process State** - Running processes, loaded DLLs, handles

### Memory Artifacts

| Artifact | Information Provided |
|----------|---------------------|
| Process List | Running executables, PIDs, parent relationships |
| Network Connections | Active TCP/UDP connections, listening ports |
| Registry Hives | Currently loaded registry keys |
| DLLs | Loaded libraries, injection detection |
| Handles | Open files, registry keys, network sockets |
| Command History | Recent commands executed |

---

## 📊 Investigation Results

### Timeline Reconstruction

**7/12/2021 4:24:52 AM** - System boot  
**7/12/2021 6:42:43 AM** - `hooker.exe` process started  
**7/12/2021 ~6:45 AM** - Network connection established to C2 server  
**7/12/2021 ~7:00 AM** - Registry keys accessed (persistence)  
**7/12/2021 ~7:15 AM** - Keylogging activity detected  

### Malware Behavior Summary

**Initial Execution:**
- Launched by user or exploit
- PID assigned: 620

**Persistence:**
- Modified registry Run keys
- Accessed NTUSER.DAT (user profile)

**Data Collection:**
- Keylogger active
- Monitored application windows
- Captured typed URLs

**Exfiltration:**
- Established C2 connection
- Remote IP: 205.134.253.10:4444
- Protocol: TCP

---

## 💡 Best Practices

### Memory Acquisition
✅ Capture memory ASAP (volatile data)  
✅ Use multiple tools for redundancy  
✅ Document system state before capture  
✅ Hash memory dump file  
✅ Store on separate media  

### Analysis Workflow
✅ Start with process list overview  
✅ Identify suspicious processes  
✅ Check network connections  
✅ Examine registry artifacts  
✅ Look for code injection  
✅ Cross-reference with disk artifacts  

### Indicators of Malware
🚨 Processes with no parent  
🚨 Unusual network connections  
🚨 Registry persistence mechanisms  
🚨 High entropy executables  
🚨 Processes hidden from standard tools  
🚨 Odd process names/locations  

---

## ⚠️ Common Mistakes

❌ Not capturing memory immediately (data lost)  
❌ Running analysis tools on live system (contamination)  
❌ Trusting process names (easily spoofed)  
❌ Ignoring network artifacts  
❌ Not documenting volatile state  
❌ Using wrong Volatility profile  

---

## 🎓 Skills Demonstrated

- ✅ Memory acquisition (multiple methods)
- ✅ Process enumeration & analysis
- ✅ Malware identification
- ✅ Hidden process detection
- ✅ Network connection analysis
- ✅ Registry artifact analysis
- ✅ Volatility Framework usage
- ✅ Entropy-based detection
- ✅ Incident timeline reconstruction

---

## 🔬 Advanced Techniques

### DLL Injection Detection
```bash
volatility -f memory.dmp --profile=Win10x64 malfind
```

### YARA Scanning
```bash
volatility -f memory.dmp --profile=Win10x64 yarascan -Y "rule malware { strings: $a = \"hooker\" condition: $a }"
```

### Command History
```bash
volatility -f memory.dmp --profile=Win10x64 cmdscan
volatility -f memory.dmp --profile=Win10x64 consoles
```

---

## 📖 References

- Volatility Command Reference
- Windows Process Reference
- Malware Analysis Fundamentals
- Memory Forensics with Volatility

## 🔗 Related Labs

- [Lab 03: Incident Response](../03-incident-response/)
- [Lab 04: Windows Forensics](../04-windows-forensics/)

---

**Time on Task:** 1 hour, 39 minutes  
**Completion:** 72%  
**Date:** April 22, 2026

**Key Evidence:**  
🚨 **hooker.exe confirmed as malware**  
🚨 **C2 connection: 205.134.253.10:4444**  
🚨 **Active keylogging detected**
