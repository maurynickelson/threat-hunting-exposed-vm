# ⭐ 01 — Preparation Phase

## 📝 Objective
Define the threat hunting scope, goals, and hypothesis before collecting or analyzing any data. This ensures the hunt remains focused and actionable.

---

## 🔍 Hunt Goal
Investigate whether any virtual machines in the shared services cluster (DNS, DHCP, AD, etc.) were **accidentally exposed to the public internet**, and determine:

- Whether the device received brute-force login attempts  
- Whether any external IPs successfully authenticated  
- Whether any unauthorized access occurred  
- Whether the legitimate user account (`notengo`) was misused  

---

## 🧠 Hypothesis
During the period when devices were mistakenly exposed, one or more internet-based threat actors **may have attempted or succeeded in brute-forcing credentials**, especially because some older systems lacked account-lockout protections.

If true, we expect to find:

- High volumes of failed logon attempts  
- Attempts from foreign or suspicious IPs  
- Possible successful authentication  
- Signs of lateral movement  
- Abnormal behavior from legitimate accounts  

---

## 🧰 Required Tools / Data Sources
- Microsoft Defender XDR / Microsoft 365 Defender  
- DeviceLogonEvents table  
- DeviceInfo table  
- Azure VM NSG audit logs (optional)  
- Public IP metadata of the VM  
- Screenshots documenting results  

---

## ✔ Expected Outcome
By completing this hunt, we will determine:

- Whether the VM was brute-forced  
- Whether any attackers succeeded  
- Whether any unauthorized account activity occurred  
- Whether the system remains secure  


