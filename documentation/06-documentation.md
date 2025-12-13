# ⭐ 06 — Documentation

## 📝 Purpose

This section brings together all the notes, screenshots, queries, and files used throughout the threat hunt. The goal is to keep everything organized in one place so the investigation can be easily followed, understood, and repeated later if needed. Good documentation makes it easier to review the hunt, explain the steps taken, and reference specific findings.

---

## 📁 What’s Included

All project files are organized into folders to keep things clean and easy to navigate:

### 🔹 Markdown Files (Full Hunt Write-up)
- `01-preparation.md`
- `02-data-collection.md`
- `03-data-analysis.md`
- `04-investigation.md`
- `05-response.md`
- `06-documentation.md` (this file)
- `07-lessons-learned.md`

These walk through each phase of the hunt from planning → investigation → remediation.

---

## 📸 Evidence & Screenshots

All screenshots used during the hunt are stored here:

```
/images/analysis/
    device-internet-facing.png
    failed-logons.png
    suspicious-ip-success-check.png
    Successful-Logons-notengo.png
    Failed-Attempts-notengo.png
```

These images include:

- Proof the VM was exposed  
- Failed logon attempts  
- Checks for successful logons  
- Activity from the legitimate user  
- Verification of login failure patterns  

Each screenshot is referenced in the documentation where it’s relevant.

---

## 🔎 KQL Queries

All KQL queries used during the investigation are stored in the `kql` folder:

```
/kql/
    failed-logons.kql
    suspicious-ip-success.kql
    correlation-check.kql
    legit-user-success.kql
    legit-user-failed.kql
```

Each file includes the exact query used so the analytics portion of the hunt can be reproduced without guessing or rewriting anything.

---

## 🔄 How Everything Connects

Each documentation file ties back to specific evidence or queries:

| Phase | File | What’s Covered |
|-------|------|-----------------|
| Preparation | 01-preparation.md | Hunt scope and hypothesis |
| Data Collection | 02-data-collection.md | Exposure verification and raw logs |
| Data Analysis | 03-data-analysis.md | All log findings + screenshots |
| Investigation | 04-investigation.md | Deep dive into what the data means |
| Response | 05-response.md | Steps taken to secure the system |
| Documentation | 06-documentation.md | This file |
| Lessons Learned | 07-lessons-learned.md | What can be improved |

This layout makes the project read like a real SOC case study.

---

## 🧪 Reproducibility

Anyone with access to similar Defender XDR data should be able to repeat the hunt by:

1. Using the queries in `/kql`
2. Following the steps outlined in each markdown file  
3. Reviewing the screenshots for expected results  
4. Comparing findings to the documented evidence  

The goal was to make the project easy to follow for other analysts or for myself later on.

---

## 🗂 Version Tracking

- **Created:** December 2025  
- **Last Updated:** December 2025  
- **Author:** Maury Nickelson  

I’ll update this documentation when new findings, screenshots, detections, or improvements are added. This helps keep the project accurate and prevents outdated information from lingering in the repo.

---

## ⭐ Conclusion

This file serves as the “index” for all evidence and notes used during the threat hunt. Everything—from screenshots to KQL queries—is stored in clearly labeled folders so it’s easy to trace how the investigation progressed and how the final conclusions were reached. The goal is to keep the entire hunt organized, clear, and usable for future reference.






