# ⭐ 06 — Documentation

## 📝 Objective
Record all artifacts, evidence, queries, screenshots, and outputs used throughout the threat hunting project. This ensures the investigation is reproducible and transparent.

Good documentation demonstrates:
- Clear investigative process  
- Repeatability  
- Traceability of findings  
- Professional SOC workflow  

---

# 📘 Summary of What Was Documented

### ✔ Preparation
- Hunt goals  
- Hypothesis  
- Scope of investigation  
- Required tools and data sources  

---

### ✔ Data Collection
- Validation that the VM was internet-facing  
- DeviceInfo logs  
- Authentication logs from DeviceLogonEvents  
- Relevant timestamps  
- Screenshot evidence of exposure  

---

### ✔ Data Analysis
- Failed logon investigations  
- Suspicious IP analysis  
- Correlation between failed and successful logons  
- Behavior of legitimate user (`notengo`)  
- Evidence screenshots for each query  
- Clear conclusions showing no successful compromise  

---

### ✔ Investigation
- Whether attackers succeeded  
- Whether attempted logons matched legitimate behavior  
- Whether suspicious IPs authenticated  
- Whether the legitimate account behaved normally  
- Whether there were signs of lateral movement  

---

### ✔ Response Actions
- Removal of public exposure  
- Hardening of authentication  
- Validation of system integrity  
- Monitoring recommendations  

---

### ✔ Evidence Stored
All images, logs, and screenshots supporting the investigation are stored inside the repository.

---

# 🗂 Repository Structure

```
/documentation
    01-preparation.md
    02-data-collection.md
    03-data-analysis.md
    04-investigation.md
    05-response.md
    06-documentation.md
    07-lessons-learned.md

/images
    /analysis
        device-internet-facing.png
        failed-logons.png
        suspicious-ip-success-check.png
        Successful-Logons-notengo.png
        Failed-Attempts-notengo.png
```

---

# 📸 Image Documentation

### ✔ Exposure Confirmation
`/images/analysis/device-internet-facing.png`  
Used in: **02-data-collection.md**

### ✔ Failed Logon Attempts
`/images/analysis/failed-logons.png`  
Used in: **03-data-analysis.md**, Step 1

### ✔ Suspicious IP

