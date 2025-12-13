# ⭐ 04 — Investigation

## 📝 Objective

The goal of this phase is to determine whether the exposed VM (`notengocyberlab`) experienced any successful compromise during the time it was publicly accessible.  
This includes:

- Investigating authentication behavior  
- Validating whether suspicious IPs successfully authenticated  
- Reviewing legitimate user activity for abnormalities  
- Identifying signs of lateral movement, privilege escalation, or persistence  
- Confirming or ruling out system compromise  

This step builds on the earlier data analysis by applying investigative reasoning rather than purely reviewing output.

---

# 🔍 Step 1 — Evaluate Authentication Behavior

Based on the data analysis phase, the VM experienced:

- Multiple failed logon attempts from external IP addresses  
- Repeated brute-force behavior against common usernames  
- No direct failed attempts against the actual legitimate account (`notengo`)  

These patterns indicate automated brute-force tools, which typically:

✔ Attempt password spraying  
✔ Do **not** know the real username  
✔ Repeatedly target common login names  
✔ Cycle through IP-based authentication probes  

### 🔎 Evidence Screenshot  
![Failed Logons](/images/analysis/failed-logons.png)

This confirms that the device was aggressively targeted but does not confirm compromise.

---

# 🔍 Step 2 — Investigate Suspicious IP Activity

We next evaluated whether any of the suspicious IP addresses:

- Successfully authenticated  
- Initiated further activity  
- Triggered alerts related to lateral movement or persistence  
- Communicated with the device after authentication attempts  

### ✔ Findings  
- **None** of the suspicious IPs authenticated successfully  
- **No** suspicious IP initiated follow-up activity  
- **No** suspicious IP appears in the successful logons dataset

### 🔎 Evidence Screenshot  
![No Suspicious Success](/images/analysis/suspicious-ip-success-check.png)

This strongly indicates attackers did not gain access.

---

# 🔍 Step 3 — Validate Legitimate User Activity

The next investigative step focuses on ensuring the legitimate user (`notengo`) did not:

- Authenticate from unusual IPs  
- Log in at abnormal times  
- Perform excessive or unexpected authentications  
- Exhibit suspicious patterns consistent with account compromise  

### ✔ Findings

- All successful logins for `notengo` originated from expected internal/management IPs  
- Logon times aligned with typical user behavior  
- Authentication count was normal  
- No geographic anomalies  
- No spikes in logon activity  

### 🔎 Evidence Screenshot  
![Successful Logons](/images/analysis/Successful-Logons-notengo.png)

No investigative flags arose from legitimate user behavior.

---

# 🔍 Step 4 — Look for Failed Attempts Against the Real Username

Targeted attempts against a real username (`notengo`) can indicate:

- Reconnaissance  
- Credential harvesting  
- Username enumeration  
- Pre-attack probing  

### ✔ Findings

Attackers did **not** attempt the legitimate username even once.

### 🔎 Evidence Screenshot  
![Failed Attempts Against User](/images/analysis/Failed-Attempts-notengo.png)

This further reinforces the conclusion that attackers did not have user-specific knowledge and were likely performing broad, automated brute-force attempts.

---

# 🔍 Step 5 — Look for Evidence of Lateral Movement or Post-Compromise Activity

Even if an attacker had successfully authenticated, typical post-compromise behaviors include:

- Remote process creation  
- Scheduled task creation  
- RDP/SMB lateral movement  
- Suspicious command-line execution  
- Privilege escalation attempts  
- Remote service manipulation  

### ✔ Findings  
A full review of available telemetry found:

- **No unusual process creations**  
- **No remote session indicators**  
- **No privilege escalation events**  
- **No new scheduled tasks**  
- **No registry modifications**  
- **No credential dumping activity**  
- **No lateral movement attempts**  

Based on all evidence, no indicators of compromise (IOCs) were present.

---

# 🧰 MITRE ATT&CK Mapping (Investigation Phase)

| Tactic | Technique | Description |
|--------|-----------|-------------|
| **Initial Access** | **T1110 — Brute Force** | Attackers attempted repeated external logons |
| **Credential Access** | **T1078 — Valid Accounts** | Confirmed no unauthorized use of valid accounts |
| **Discovery** | **T1087 — Account Discovery** | Attackers attempted login enumeration |
| **Execution / Lateral Movement** | *(None detected)* | No signs of post-authentication activity |

---

# 🧾 Summary of Investigation Findings

| Question | Result |
|---------|--------|
| Did attackers brute-force the device? | ✔ Yes — many failed attempts |
| Did any suspicious IP log in successfully? | ❌ No |
| Did legitimate user behavior look abnormal? | ✔ Normal activity only |
| Was the legitimate account targeted? | ❌ No attempts on “notengo” |
| Any lateral movement? | ❌ None |
| Any privilege escalation? | ❌ None |
| Any persistence attempts? | ❌ None |
| Any indicators of compromise? | ❌ No evidence of compromise |

---

# ⭐ Conclusion

Based on log evidence, suspicious IP analysis, legitimate user validation, and behavioral review:

## **➡ The VM was attacked but NOT compromised.**

External entities attempted brute-force activity during the exposure period,  
but none succeeded in authentication or follow-up activity.

The system remains considered **secure and uncompromised**.



