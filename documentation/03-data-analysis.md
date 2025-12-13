# ⭐ 03 — Data Analysis

## 📝 Objective

The purpose of this phase is to analyze authentication logs from the exposed VM (`notengocyberlab`) to determine:

- Whether brute-force attempts occurred  
- Whether any suspicious IPs successfully authenticated  
- Whether the legitimate user account (`notengo`) showed anomalies  
- Whether any activity indicates a compromise  

This analysis validates whether the system was attacked, breached, or remained secure.

---

# 🕒 Analysis Time Window

All analysis was performed on the exposure window identified during **Data Collection**:

**➡ 2025-12-01 through 2025-12-11**

This ensures all queries evaluate activity **only** during the period when the VM was internet-facing.

---

# 📁 Data Sources Analyzed

We analyzed the following Defender XDR log sources:

- **DeviceLogonEvents** — Failed & successful authentication attempts  
- **DeviceInfo** — Exposure status, system metadata  
- **Remote IP telemetry** — Source addresses attempting authentication  

These datasets form the evidence base for identifying attempted and successful intrusions.

---

# 🔍 Step 1 — Identify Failed Logon Attempts

Before determining whether any breaches occurred, we first check for brute-force behavior targeting the VM.

### ✔ Why This Matters
Brute-force attacks show up as repeated failed login attempts from the same remote IPs.

### 🔎 KQL Query: Failed Logons

```kql
DeviceLogonEvents
| where LogonType has_any("Network", "Interactive", "RemoteInteractive", "Unlock")
| where ActionType == "LogonFailed"
| where isnotempty(RemoteIP)
| summarize Attempts = count() by ActionType, RemoteIP, DeviceName
| order by Attempts desc
```

### 📸 Screenshot — Failed Logons  
![Failed Logons](/images/analysis/failed-logons.png)

This confirms **multiple repeated failed logins** from external IPs, indicating brute-force attempts.

---

# 🔍 Step 2 — Check Whether Suspicious IPs Ever Logged In

Next, we check whether those same suspicious IPs were ever able to successfully authenticate.

### ✔ Why This Matters
If even **one** suspicious IP authenticated successfully → **confirmed compromise**.

### 🔎 KQL Query: Suspicious IP Success Check

```kql
let RemoteIPsInQuestion = dynamic(["186.10.23.226", "119.42.115.235","183.81.169.238", "74.39.190.50", "121.30.214.172", "83.222.191.62", "45.41.204.12", "192.109.240.116"]);
DeviceLogonEvents
| where LogonType has_any("Network", "Interactive", "RemoteInteractive", "Unlock")
| where ActionType == "LogonSuccess"
| where RemoteIP has_any(RemoteIPsInQuestion)
```

### 📸 Screenshot — No Successful Logons from Suspicious IPs  
![Suspicious IP Success Check](/images/analysis/suspicious-ip-success-check.png)

This confirms **none of the suspicious IPs successfully authenticated**.

---

# 🔍 Step 3 — Compare Failed vs. Successful Logons (Correlation Check)

Here we look for any IP that:

1. Failed multiple times  
2. Eventually succeeded  

This would strongly indicate a brute-force compromise.

### 🔎 KQL Query: Failed vs Successful Correlation

```kql
let FailedLogons = DeviceLogonEvents
| where LogonType has_any("Network", "Interactive", "RemoteInteractive", "Unlock")
| where ActionType == "LogonFailed"
| where isnotempty(RemoteIP)
| summarize FailedLogonAttempts = count() by ActionType, RemoteIP, DeviceName;

let SuccessfulLogons = DeviceLogonEvents
| where LogonType has_any("Network", "Interactive", "RemoteInteractive", "Unlock")
| where ActionType == "LogonSuccess"
| where isnotempty(RemoteIP)
| summarize SuccessfulLogons = count() by ActionType, RemoteIP, DeviceName, AccountName;

FailedLogons
| join kind=leftouter SuccessfulLogons on RemoteIP
| project RemoteIP, DeviceName, FailedLogonAttempts, SuccessfulLogons, AccountName
```

### 📸 Screenshot — Correlation Results  
![Correlation Results](/images/analysis/suspicious-ip-success-check.png)

No suspicious IP appears in both failed + successful lists.

---

# 🔍 Step 4 — Analyze Successful Logons for Legitimate User “notengo”

Now we validate whether the real account used for this VM shows any anomalies.

### ✔ Why This Matters
Even if attackers didn’t brute-force successfully, they may attempt credential stuffing or targeted login attempts.

### 🔎 KQL Query: Legitimate User Successful Logons

```kql
DeviceLogonEvents
| where DeviceName == "notengocyberlab"
| where LogonType == "Network"
| where ActionType == "LogonSuccess"
| where AccountName == "notengo"
| summarize LoginCount = count() by DeviceName, ActionType, AccountName, RemoteIP
```

### 📸 Screenshot — Successful Logons by notengo  
![Successful Logons](/images/analysis/Successful-Logons-notengo.png)

These logons appear normal and originate from expected internal or management IPs.

---

# 🔍 Step 5 — Check for Failed Attempts Against “notengo”

This checks whether attackers attempted logging in **with the real username**, indicating reconnaissance or targeted guessing.

### 🔎 KQL Query: Failed Attempts for Account “notengo”

```kql
DeviceLogonEvents
| where DeviceName == "notengocyberlab"
| where LogonType == "Network"
| where ActionType == "LogonFailed"
| where AccountName == "notengo"
| summarize count()
```

### 📸 Screenshot — Failed Attempts Against notengo  
![Failed Attempts](/images/analysis/Failed-Attempts-notengo.png)

Attackers **did not attempt** the legitimate username — all attempts targeted generic username guesses.

---

# 🧰 MITRE ATT&CK Mapping

| Tactic | Technique | Details |
|--------|-----------|---------|
| **Initial Access** | **T1110 — Brute Force** | Multiple repeated failed remote login attempts from external IPs |
| **Credential Access** | **T1078 — Valid Accounts** | Analysis confirms no unauthorized valid account usage |
| **Discovery** | **T1087 — Account Discovery** | Attackers attempted to identify valid usernames based on login failures |

---

# 🧾 Summary of Analysis Results

| Question | Result |
|---------|--------|
| Were brute-force attempts detected? | ✔ Yes — multiple repeated failed logons |
| Did any suspicious IP succeed? | ❌ No successful logons |
| Any failed → successful correlation? | ✔ None detected |
| Any anomalies in legitimate user logons? | ✔ Legitimate activity only |
| Was targeted username guessing attempted? | ❌ No attempts on “notengo” |
| Was the system compromised? | ❌ No indicators of compromise |

---

# ⭐ Conclusion

The system was aggressively targeted by external brute-force attempts, but:

- No attacker authenticated successfully  
- No suspicious IPs correlated across failed/ successful events  
- Legitimate account activity was normal  
- No evidence of compromise was found  

This concludes that the VM was **attacked but NOT breached**.



