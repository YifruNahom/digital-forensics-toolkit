# Lab 01: Chain of Custody & Daubert Standard

## 🎯 Objective
Learn proper evidence handling procedures, chain of custody documentation, and hash-based file verification using forensic tools.

## 📚 Topics Covered
- Daubert Standard for forensic evidence
- Chain of custody procedures
- Evidence file extraction
- Cryptographic hash verification
- File integrity validation

## 🛠 Tools Used
- **Adobe Reader** - Search warrant documentation
- **FTK Imager** - Evidence file extraction & hash generation
- **Paraben's E3** - Hash verification

## 📝 Lab Procedures

### Part 1: Chain of Custody Documentation

**Steps:**
1. Open search warrant in Adobe Reader
2. Review warrant contents (Case No. 10081-BFD-CCD)
3. Complete Chain of Custody form with:
   - Collector signature & date
   - Copy method (physical media, logical file copy)
   - Disposition tracking
   - Transfer history

**Key Learning:** Proper documentation is critical for evidence admissibility in court.

### Part 2: Evidence Extraction with FTK Imager

**Steps:**
1. Launch FTK Imager
2. Add evidence source (USB drive)
3. Navigate to evidence files
4. Export files with hash generation:
   - `0002665.jpg`
   - `RecycleBinEvidence.docx`
   - `MyRussianMafiaBuddies.txt`
   - `Nice guys.png`

**Hash Files Generated:**
- `0002665_hash.csv`
- `RecycleBinEvidence_hash.csv`
- `MyRussianMafiaBuddies_hash.csv`
- `Nice guys_hash.csv`

**Sample Hash Output:**
```csv
MD5,SHA1,Filename
"e4cac1e59054af797bc~fe67f6f7d97d7","374a07033aac5bde0ea47ea1cce9e77fd7dbfa","Evidence.jpg"
```

### Part 3: Hash Verification with E3

**Steps:**
1. Open E3 (Electronic Evidence Examiner)
2. Load extracted files
3. Calculate MD5 and SHA1 hashes
4. Compare with FTK Imager hashes
5. Verify integrity

**Question:** Do the hash values match between tools?
**Answer:** Yes, because both tools use the same hashing algorithms (MD5, SHA-1). This proves file integrity.

## 🔑 Key Concepts

### Daubert Standard
The Daubert Standard is a legal precedent for admitting expert testimony based on scientific evidence. Forensic evidence must be:
- Based on testable methodology
- Peer-reviewed
- Have known error rates
- Be generally accepted in the field

### Chain of Custody
Chronological documentation showing:
- Who collected evidence
- When & where it was collected
- Who had possession
- How it was transferred
- Where it was stored

**Why it matters:** Broken chain of custody can result in evidence being inadmissible in court.

### Cryptographic Hashing
- **MD5** - 128-bit hash (considered weak for security, but acceptable for forensic verification)
- **SHA-1** - 160-bit hash (stronger than MD5)
- **Purpose** - Proves file hasn't been altered (integrity verification)

## 📊 Results

### Evidence Files Processed
| File Name | Size | MD5 Hash | SHA-1 Hash |
|-----------|------|----------|------------|
| 0002665.jpg | 1.4 MB | [32-char hex] | [40-char hex] |
| RecycleBinEvidence.docx | 4.2 MB | [32-char hex] | [40-char hex] |
| MyRussianMafiaBuddies.txt | ~2 KB | [32-char hex] | [40-char hex] |
| Nice guys.png | ~800 KB | [32-char hex] | [40-char hex] |

### Verification Status
✅ All files verified - Hashes match between FTK Imager and E3

## 💡 Best Practices

1. **Always use write-blockers** when accessing original evidence
2. **Generate hashes immediately** after acquisition
3. **Document everything** - dates, times, personnel
4. **Use multiple hash algorithms** for redundancy
5. **Verify hashes** before and after file transfers
6. **Store original evidence** separately from working copies

## ⚠️ Common Mistakes

- ❌ Not documenting evidence transfer
- ❌ Working directly on original evidence
- ❌ Failing to generate hashes
- ❌ Incomplete chain of custody forms
- ❌ Not securing evidence storage

## 🎓 Skills Demonstrated

- ✅ Legal evidence handling
- ✅ Chain of custody documentation
- ✅ Forensic tool proficiency (FTK Imager, E3)
- ✅ Hash generation & verification
- ✅ File integrity validation
- ✅ Evidence documentation

## 📖 References

- Federal Rules of Evidence
- NIST Computer Forensics Guidelines
- Daubert v. Merrell Dow Pharmaceuticals (1993)
- Digital Forensics, Investigation, and Response (4th Ed.)

## 🔗 Related Labs

- [Lab 03: Incident Response Investigation](../03-incident-response/)
- [Lab 04: Windows Forensics](../04-windows-forensics/)

---

**Time on Task:** 1 hour, 3 minutes  
**Completion:** 52%  
**Date:** January 28, 2026
