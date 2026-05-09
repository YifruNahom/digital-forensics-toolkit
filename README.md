# Digital Forensics Toolkit 🔍

> A comprehensive collection of digital forensics tools, techniques, and methodologies learned through hands-on lab work

[![GitHub](https://img.shields.io/badge/GitHub-nahomyifru-blue)](https://github.com/YifruNahom)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## 📋 Table of Contents

- [Overview](#overview)
- [Repository Structure](#repository-structure)
- [Core Competencies](#core-competencies)
- [Tools & Technologies](#tools--technologies)
- [Lab Exercises](#lab-exercises)
- [Getting Started](#getting-started)
- [Key Learnings](#key-learnings)
- [Resources](#resources)
- [Contact](#contact)

## 🎯 Overview

This repository documents practical experience in digital forensics, incident response, and cybersecurity investigations. It covers evidence handling, file recovery, system analysis, memory forensics, network investigation, and mobile device examination.

**Student:** Nahom Yifru  
**Institution:** Bergen Community College  
**Email:** nyifru@me.bergen.edu  
**Course:** Digital Forensics, Investigation, and Response (4th Edition)

## 📁 Repository Structure

```
digital-forensics-toolkit/
├── 01-chain-of-custody/          # Evidence handling & legal standards
├── 02-file-recovery/             # Deleted & damaged file recovery
├── 03-incident-response/         # IR investigation procedures
├── 04-windows-forensics/         # Windows system analysis
├── 05-memory-forensics/          # RAM analysis & malware detection
├── 06-network-forensics/         # Network traffic & infrastructure
├── 07-mobile-forensics/          # iOS & Android investigations
├── tools/                        # Tool documentation & scripts
├── reports/                      # Sample forensic reports
└── resources/                    # Reference materials
```

## 🎓 Core Competencies

### Legal & Evidence Management
- Chain of custody procedures
- Daubert Standard application
- Evidence documentation
- Search warrant handling

### File Systems & Data Recovery
- NTFS, Ext4, FAT32, APFS analysis
- Deleted file recovery
- File carving techniques
- Hash verification (MD5, SHA-1)

### System Forensics
- Windows Registry analysis
- Process memory examination
- User activity reconstruction
- Artifact collection

### Network Analysis
- Packet capture & analysis (Wireshark)
- Router/firewall forensics
- Network traffic filtering
- Protocol analysis

### Incident Response
- Malware detection
- Keylogger identification
- Data exfiltration investigation
- Timeline reconstruction

## 🛠 Tools & Technologies

### Evidence Acquisition
- **FTK Imager** - Forensic imaging & file extraction
- **DumpIt** - Memory capture utility
- **dd** - Disk imaging (Linux)

### Analysis Tools
- **Paraben's E3** - Electronic Evidence Examiner
- **Autopsy** - Open-source digital forensics platform
- **Wireshark** - Network protocol analyzer
- **Volatility** - Memory forensics framework
- **PhotoRec** - File recovery tool
- **Registry Editor** - Windows registry analysis

### Mobile Forensics
- **Paraben's E3** - iOS & Android data extraction
- iOS backup analysis
- Android SQLite database examination

### Scripting & Automation
- **PowerShell** - Windows forensic scripting
- **Bash** - Linux command-line forensics
- **Python** - Automation scripts

### Hashing & Verification
- **MD5** - File integrity verification
- **SHA-1** - Cryptographic hash functions
- **DensityScout** - Malware detection

## 🔬 Lab Exercises

### Lab 1: Chain of Custody & Legal Standards
- ✅ Search warrant documentation
- ✅ Chain of custody form completion
- ✅ Evidence file extraction with FTK Imager
- ✅ Hash code generation & verification

### Lab 2: File Recovery
- ✅ NTFS deleted file recovery with E3
- ✅ Ext4 file recovery with Autopsy
- ✅ PhotoRec usage for Linux systems
- ✅ RAR archive extraction

### Lab 3: Incident Response
- ✅ PCAP file analysis
- ✅ Email evidence extraction
- ✅ FTP credential discovery
- ✅ Keylogger detection

### Lab 4: Windows Forensics
- ✅ Process enumeration
- ✅ Registry analysis (Winlogon, ShellBags, RecentDocs)
- ✅ Browser history examination
- ✅ File system timeline creation

### Lab 5: Memory Forensics
- ✅ Memory capture with DumpIt & FTK Imager
- ✅ Process analysis with E3
- ✅ Volatility framework usage
- ✅ Hidden process detection
- ✅ Network connection analysis

### Lab 6: Network Forensics
- ✅ Packet capture with Wireshark
- ✅ Router forensics (Cisco IOS)
- ✅ Firewall log analysis (pfSense)
- ✅ FTP traffic reconstruction
- ✅ HTTP stream analysis

### Lab 7: Mobile Device Forensics
- ✅ iOS backup analysis
- ✅ Android user data extraction
- ✅ Contact & message recovery
- ✅ Application data examination
- ✅ Timeline reconstruction

## 🚀 Getting Started

### Prerequisites
```bash
# Windows Tools
- FTK Imager
- Paraben's E3
- Wireshark

# Linux Tools
- Autopsy
- PhotoRec
- Volatility

# General
- VirtualBox/VMware for lab environments
```

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/YifruNahom/digital-forensics-toolkit.git
cd digital-forensics-toolkit
```

2. **Review tool documentation**
```bash
cd tools/
cat TOOLS.md
```

3. **Explore lab exercises**
```bash
cd 01-chain-of-custody/
cat README.md
```

## 💡 Key Learnings

### Evidence Handling
- Always maintain proper chain of custody
- Document every step of the investigation
- Use write-blockers for evidence acquisition
- Generate and verify hash values

### File Recovery
- Multiple tools increase recovery success
- File carving can recover files without metadata
- NTFS MFT contains valuable recovery information
- Deleted ≠ Gone (until overwritten)

### Incident Response
- Timeline reconstruction is critical
- Look for persistence mechanisms
- Network artifacts reveal C2 communications
- Memory contains evidence not on disk

### Windows Forensics
- Registry is a goldmine of user activity
- ShellBags track folder access
- RecentDocs shows file interactions
- Prefetch files indicate program execution

### Memory Forensics
- Volatile memory captures running state
- Hidden processes often indicate malware
- Network connections reveal communications
- DLL injection is a common malware technique

### Network Forensics
- Packet captures preserve network evidence
- Follow TCP Stream for session reconstruction
- Router configs contain routing & access info
- Firewall logs track blocked connections

### Mobile Forensics
- Backups contain extensive device data
- SQLite databases store app information
- Deleted records often remain in databases
- Application data reveals user behavior

## 📚 Resources

### Official Documentation
- [Autopsy User Guide](https://www.sleuthkit.org/autopsy/)
- [Volatility Documentation](https://volatility3.readthedocs.io/)
- [Wireshark User Guide](https://www.wireshark.org/docs/)

### Learning Resources
- [SANS Digital Forensics](https://www.sans.org/cyber-security-courses/advanced-incident-response-threat-hunting-training/)
- [NIST Computer Forensics Tool Testing](https://www.nist.gov/itl/ssd/software-quality-group/computer-forensics-tool-testing-program-cftt)

### Books
- *Digital Forensics, Investigation, and Response* (4th Edition)

## 📫 Contact

**Nahom Yifru**
- Email: yifrunahom@gmail.com
- LinkedIn: [linkedin.com/in/nahom-yifru-15871b278](https://www.linkedin.com/in/nahom-yifru-15871b278)
- GitHub: [@YifruNahom](https://github.com/YifruNahom)
- Phone: (301) 728-8885
- Location: Northern New Jersey

## 📄 License

This repository is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Bergen Community College Cybersecurity Program
- Digital Forensics, Investigation, and Response course instructors
- Open-source forensics community

---

⭐ **If you find this repository helpful, please consider giving it a star!**
