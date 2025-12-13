# ⭐ 04 — Investigation

## 📝 Objective
Investigate findings from the analysis phase to determine whether the exposed VM experienced a successful compromise, unauthorized access, or malicious behavior.

This phase focuses on validating what the logs *mean*, understanding attacker behavior, and confirming whether any suspicious activity progressed beyond failed authentication attempts.

---

# 🔍 Step 1 — Investigate High-Volume Failed Logons

Using the failed logon data collected earlier, the first step is to determine whether the volume and nature of failed attempts indicate automated brute-forcing.

### ✔ What was investigated
- IP addresses with repeated failures  
- Geographic locations of attacking IPs  
- Whether attacks came from known malicious hosts  
- Whether attacks increased over time  

### 📸 Evidence  
![Failed Logons Output](/images/analysis/failed-logons.png)

### 🧠 Interpretation
- The device received numerous failed login attempts from globally distributed IPs.  
- This pattern is consistent with **automated brute-force attacks**, not a targeted human adversary.  

---

# 🔍 Step 2 — Investigate Successful Logons

This step verifies whether any unexpected or suspicious IP addresses successfully authenticated.

### ✔ What was checked
- Whether any suspicious IP showed up in successful logons  
- Whether successful events matched only known/legitimate user behavior  
- Whether logons occurred at unusual times  

### 📸 Evidence  
![Suspicious IP Success Check](/images/analysis/suspicious-ip-success-check.png)

### 🧠 Interpretation
- No suspicious IPs successfully authenticated.  
- All successful logons came from the legitimate account (`notengo`) only.  

---

# 🔍 Step 3 — Investigate Legitimate User Behavior

Next, validate whether the legitimate account (`notengo`) behaved normally:

### ✔ What was checked
- Geographic source of normal logons  
- Frequency of normal logons  
- RemoteIP values used by `notengo`  
- Any signs of credential misuse  

### 📸 Evidence  
![Notengo Successful Logons](/images/analysis/Successful-Logons-notengo.png)

### 🧠 Interpretation
- All legitimate logons originated from expected IPs.  
- No logons came from unusual locations or anonymous networks.  
- Logon frequency matched typical system behavior.  

---

# 🔍 Step 4 — Correlate Failed + Successful Attempts

To ensure no adversary slipped through:

### ✔ What correlation means  
If an IP appears in BOTH:
- **Failed logons**, and  
- **Successful logons**,  

…it suggests the attacker eventually guessed the password.

### 📸 Evidence  
![Failed vs Successful Correlation](/images/analysis/suspicious-ip-success-check.png)

### 🧠 Interpretation
- No attacking IPs appear in both categories.  
- There is **no evidence** that a brute-force attack succeeded.  

---

# 🔍 Step 5 — Investigate Failed Attempts Targeting Legitimate User

Attackers sometimes target the real username after fingerprinting the device.

### ✔ What was investigated
- Did anyone attempt to login as `notengo`?  
- Was the username enumerated?  
- Was targeted brute-forcing attempted?  

### 📸 Evidence  
![Notengo Failed Attempts](/images/analysis/Failed-Attempts-notengo.png)

### 🧠 Interpretation
- No attackers attempted to use the real username.  
- Attackers attempted generic usernames instead.  
- This suggests automated, untargeted scanning.  

---
---

# 🔗 MITRE ATT&CK Mapping

Based on the observed authentication patterns and failed login attempts, the following MITRE ATT&CK techniques apply:

### 🟧 **T1110 — Brute Force**
Attackers attempted repeated authentication using multiple remote IP addresses, consistent with automated credential-guessing tools.

### 🟧 **T1110.001 — Password Guessing**
The failed logon attempts used common brute-force usernames and high-frequency retries.

### 🟦 **T1021.001 — Remote Services: RDP**
The exposed RDP port (3389) enabled attackers to attempt remote authentication directly.

### 🟦 **T1078 — Valid Accounts** *(Not Observed but Relevant)*
This technique was evaluated because attackers attempted access.  
However, **no successful authentication occurred**, so the technique was not fully executed.

### 🟩 Techniques Ruled Out
These stages were checked for but **no evidence was found**:

- T1059 — Command Execution  
- T1105 — Ingress Tool Transfer  
- T1003 — Credential Dumping  
- T1570 — Lateral Movement  
- T1053 — Scheduled Task Persistence  

---

# 🧠 Interpretation
The activity observed corresponds to the **Initial Access** phase of the attack lifecycle.  
The adversary attempted credential access but **was unable to progress to Execution, Persistence, Privilege Escalation, or Lateral Movement** stages.


# ⭐ Investigation Summary

### After completing the investigation:

✔ Multiple external hosts attempted brute-forcing  
✔ All attempts originated from suspicious global IP addresses  
✔ **None** resulted in successful authentication  
✔ Legitimate user behavior matched normal predictable usage  
✔ No signs of lateral movement  
✔ No signs of privilege escalation  
✔ No persistence mechanisms detected  
✔ No unauthorized accounts created  

### ⭐ Final Determination  
**The device was attacked, but not compromised.**
