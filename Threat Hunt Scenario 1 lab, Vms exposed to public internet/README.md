# 🔍 Threat Hunting Lab: Exposed VM Brute-Force Investigation

## ⭐ Overview
This project documents a real-world threat hunting investigation where an Azure virtual machine was **accidentally exposed to the public internet**, resulting in multiple brute-force login attempts.

Using Microsoft Defender XDR and KQL, the investigation determined:

- Whether the VM received brute-force attempts  
- Whether any attackers successfully authenticated  
- Whether the legitimate user account (`notengo`) behaved abnormally  
- Whether the system showed signs of compromise  

This repository showcases **structured analysis, real-world SOC workflow, and professional documentation**.

---

# 🧩 Scenario Summary

A VM supporting shared services (DNS, DHCP, Domain Services) was unintentionally exposed to the internet. During that time:

- Automated attackers began brute-forcing the RDP service  
- Multiple global IPs attempted authentication  
- Some devices lacked account lockout protections  

This project documents the complete threat hunt to determine whether the exposure resulted in compromise.

---

# 🛠 Tools & Technologies

- **Microsoft Defender XDR**
- **Kusto Query Language (KQL)**
- **Microsoft Azure**
- **DeviceLogonEvents logs**
- **DeviceInfo logs**
- **Azure NSG / Firewall settings**

---

# 📁 Repository Structure

```
documentation/
    01-preparation.md
    02-data-collection.md
    03-data-analysis.md
    04-investigation.md
    05-response.md
    06-documentation.md
    07-lessons-learned.md

images/
    analysis/
        device-internet-facing.png
        failed-logons.png
        suspicious-ip-success-check.png
        Successful-Logons-notengo.png
        Failed-Attempts-notengo.png
```

---

# 📊 MITRE ATT&CK Mapping

| Technique ID | Name | Observed? | Notes |
|--------------|------|-----------|-------|
| **T1110** | Brute Force | ✔️ | Multiple failed authentication attempts |
| **T1110.001** | Password Guessing | ✔️ | Automated credential attempts |
| **T1021.001** | Remote Services: RDP | ✔️ | Attackers targeted RDP over the internet |
| **T1078** | Valid Accounts | ❌ | No successful unauthorized login observed |
| **T1059 / T1105 / T1570** | Execution / Tool Transfer / Lateral Movement | ❌ | No compromise indicators found |

---

# 📈 Key Findings

### ✔ Multiple global IPs attempted brute-force attacks  
### ✔ No suspicious IPs successfully authenticated  
### ✔ The legitimate user’s account (`notengo`) behaved normally  
### ✔ No lateral movement or persistence observed  
### ✔ VM was attacked, but **not breached**

---

# 📘 Documentation Included

Each major phase of the threat hunt is documented:

### 🔹 01-preparation  
Defining scope, hypothesis, data sources

### 🔹 02-data-collection  
Pulling logs, confirming internet exposure

### 🔹 03-data-analysis  
KQL queries, screenshots, correlation

### 🔹 04-investigation  
Interpretation, findings, MITRE mapping

### 🔹 05-response  
Hardening, remediation, monitoring

### 🔹 06-documentation  
Repository evidence, reproducibility

### 🔹 07-lessons-learned  
Reflections, improvements, takeaways

---

# 🧠 Skills Demonstrated

- Threat hunting methodology  
- KQL proficiency  
- Understanding of brute force behavior  
- MITRE ATT&CK mapping  
- Authentication log analysis  
- Incident response fundamentals  
- Cloud security (Azure)  
- Professional documentation  

---

# 🙋‍♂️ Author

Maury  
Cybersecurity practitioner, threat hunter, and SOC analyst in training.  
Passionate about hands-on labs, detection engineering, and blue team defense.

---

# ⭐ Final Summary

This investigation demonstrates a full end-to-end threat hunt:

- Defined a hypothesis  
- Collected relevant telemetry  
- Analyzed failed and successful logons  
- Investigated suspicious behavior  
- Confirmed the system was **attacked but not compromised**  
- Documented remediation steps  
- Mapped activity to MITRE ATT&CK  
- Produced professional SOC-ready documentation  

This repository serves as a complete example of a **real-world cybersecurity investigation**.

