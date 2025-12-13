# ⭐ Data Analysis

## 📝 Objective
The purpose of this phase is to analyze authentication logs from the exposed VM (`notengocyberlab`) to determine:

- Whether brute-force attempts occurred  
- Whether any suspicious IPs successfully authenticated  
- Whether the legitimate account (`notengo`) showed abnormal behavior  
- Whether the device showed signs of compromise  

This analysis helps determine if the system was actually breached or if attacks were unsuccessful.

---

# 🔍 Step 1 — Identify Failed Logon Attempts

Before checking for successful access, I first needed to confirm whether the device received a high volume of **failed login attempts**, which often indicates brute-force activity.

### ✔ Why this matters  
If attackers discovered the exposed public IP, they often flood the device with password attempts.

### 🔎 KQL Query: Failed Logons

````kql
DeviceLogonEvents
| where LogonType has_any("Network", "Interactive", "RemoteInteractive", "Unlock")
| where ActionType == "LogonFailed"
| where isnotempty(RemoteIP)
| summarize Attempts = count() by ActionType, RemoteIP, DeviceName
| order by Attempts


