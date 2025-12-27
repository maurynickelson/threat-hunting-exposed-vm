# Internet-Exposed VM Brute-Force Investigation – Defender XDR Threat Hunt

## Overview
This project documents a threat hunt conducted after a critical virtual machine (`notengocyberlab`) was mistakenly exposed to the public internet. The objective was to determine whether the exposure resulted in brute-force attacks, successful authentication, or any form of system compromise.

Using Microsoft Defender XDR telemetry, authentication logs were analyzed across the exposure window to assess attacker behavior, validate legitimate user activity, and confirm whether any indicators of compromise (IOCs) were present.

**Final Assessment:**  
➡ The system was **attacked but not compromised**.

---

## Environment & Tooling
- Microsoft Defender XDR / Microsoft 365 Defender
- Kusto Query Language (KQL)
- Primary Tables:
  - `DeviceInfo`
  - `DeviceLogonEvents`
- Supporting Evidence:
  - Authentication telemetry
  - Remote IP analysis
  - Screenshots documenting results
- MITRE ATT&CK Framework

---

## Threat Hypothesis
During the period when the VM was publicly exposed, external threat actors may have attempted brute-force authentication. Given that older systems may lack strict account-lockout protections, successful compromise was possible.

If a compromise occurred, evidence would include:
- High volumes of failed logons
- Failed → successful authentication correlation
- Unauthorized successful logons
- Abnormal behavior from the legitimate user account
- Signs of lateral movement or persistence

---

## Exposure Verification
Before analysis, exposure was confirmed to ensure the investigation focused only on the relevant time window.

```kql
DeviceInfo
| where DeviceName == "notengocyberlab"
| where IsInternetFacing == true
| order by Timestamp desc
```
Analysis Window:
2025-12-01 → 2025-12-11

---
### Failed Logon Activity
Repeated failed authentication attempts were identified from multiple external IP addresses, consistent with automated brute-force behavior.
```kql
DeviceLogonEvents
| where LogonType has_any("Network", "Interactive", "RemoteInteractive", "Unlock")
| where ActionType == "LogonFailed"
| where isnotempty(RemoteIP)
| summarize Attempts = count() by RemoteIP, DeviceName
| order by Attempts desc
```
Assessment:
✔ Brute-force attempts observed
✔ External, automated attack patterns
---

### Suspicious IP Success Validation
Suspicious IPs identified during failed attempts were checked for successful authentication.
```kql
let RemoteIPsInQuestion = dynamic([
  "186.10.23.226","119.42.115.235","183.81.169.238",
  "74.39.190.50","121.30.214.172","83.222.191.62",
  "45.41.204.12","192.109.240.116"
]);
DeviceLogonEvents
| where ActionType == "LogonSuccess"
| where RemoteIP has_any(RemoteIPsInQuestion)
```
Result:
❌ No suspicious IP successfully authenticated
---

### Failed → Successful Correlation Check
A correlation analysis was performed to identify IPs that failed repeatedly and later succeeded.
```kql
let FailedLogons = DeviceLogonEvents
| where ActionType == "LogonFailed"
| where isnotempty(RemoteIP)
| summarize FailedAttempts = count() by RemoteIP;

let SuccessfulLogons = DeviceLogonEvents
| where ActionType == "LogonSuccess"
| where isnotempty(RemoteIP)
| summarize SuccessCount = count() by RemoteIP;

FailedLogons
| join kind=leftouter SuccessfulLogons on RemoteIP
```
Result:
✔ No correlation between failed and successful attempts

---

### Legitimate User Validation
The legitimate account (notengo) was reviewed for anomalies.
```kql
DeviceLogonEvents
| where DeviceName == "notengocyberlab"
| where AccountName == "notengo"
| where ActionType == "LogonSuccess"
| summarize count() by RemoteIP
```
Findings:

- Expected IP addresses only

- Normal login frequency and timing

- No geographic anomalies

- No failed attempts targeting the legitimate username

---

### Investigation Results
Key Findings

- External brute-force attempts were detected

- Attackers targeted generic usernames

- No attacker successfully authenticated

- No lateral movement detected

- No privilege escalation

- No persistence mechanisms

- No indicators of compromise

--- 

### MITRE ATT&CK Mapping
| Tactic                       | Technique         | ID    | Evidence                                         |
| ---------------------------- | ----------------- | ----- | ------------------------------------------------ |
| Initial Access               | Brute Force       | T1110 | Repeated failed external authentication attempts |
| Credential Access            | Valid Accounts    | T1078 | Confirmed absence of unauthorized account usage  |
| Discovery                    | Account Discovery | T1087 | Username enumeration via failed logons           |
| Execution / Lateral Movement | None Detected     | —     | No post-authentication activity observed         |

## Response & Recommended Remediation

### Network Hardening
- Restrict RDP access to trusted IP addresses
- Remove public inbound access from the Network Security Group (NSG)

### Authentication Controls
- Implement account lockout thresholds
- Enable Multi-Factor Authentication (MFA)

### Security Benefits
- Prevent brute-force attacks
- Reduce attack surface
- Strengthen identity protections

---

## Lessons Learned & Security Improvements

### Key Takeaways
- **Public-facing services are attacked immediately:** Even short exposure resulted in global brute-force attempts.
- **Lack of account lockout increases risk:** Unlimited guessing enables attackers to persist.
- **Log visibility was sufficient:** Defender telemetry allowed full validation of attack activity.
- **Correlation is critical:** Failed vs successful logon correlation proved no breach occurred.
- **Legitimate user validation is essential:** Confirming expected behavior ruled out compromise.
- **Automated attacks dominate exposure events:** Most activity was non-targeted background internet scanning.

- Enable MFA for all administrative access
- Improve alerting for:
  - Repeated failed logons
  - Failed → successful authentication patterns
  - RDP access from non-approved IPs
- Regularly review NSG and firewall configurations
- Prefer VPN-based administrative access over public exposure

---

## Final Conclusion
The exposed virtual machine was actively targeted by automated brute-force attacks during the exposure window. However:

- No successful authentication occurred
- No legitimate account misuse was observed
- No post-compromise activity was detected

➡ **Final determination: Attacked but NOT compromised**

This investigation demonstrates real-world attack behavior, effective log-based analysis, and the importance of layered security controls in preventing compromise.



