# ⭐ 07 — Lessons Learned

## 📝 Objective
Summarize insights gained from the hunt, identify gaps, and document improvements to reduce risk and strengthen future detection and response.

This section demonstrates reflection, analysis maturity, and your ability to take technical findings and translate them into actionable improvements.

---

# 🔍 Key Takeaways From the Hunt

### ⭐ 1. Exposed Services Are Immediately Targeted  
The VM was exposed only briefly, yet it received multiple brute-force attempts from global IP ranges.  
This reflects real-world conditions: **public-facing RDP is scanned and attacked within minutes.**

---

### ⭐ 2. Lack of Account Lockout Enables Brute-Force Attempts  
Older systems without lockout policies allow attackers unlimited guessing attempts.  
Even though no compromise occurred, the environment was unnecessarily vulnerable.

---

### ⭐ 3. Log Visibility Was Sufficient for Detection  
Microsoft Defender's DeviceLogonEvents and DeviceInfo tables captured enough telemetry to:

- Detect brute-force attempts  
- Correlate failed vs successful logons  
- Validate legitimate vs suspicious authentication  
- Confirm no unauthorized use of the `notengo` account  

---

### ⭐ 4. Correlation Is Critical  
Simply reviewing failed or successful logons alone is not enough.  
The key insight came from **correlating failed attempts with successful ones** to prove no attacker succeeded.

---

### ⭐ 5. Legitimate User Behavior Validated System Integrity  
By validating that:

- All successful logons came from the correct user  
- All IPs used by `notengo` were expected  
- No suspicious activity followed successful logons  

…we gained high assurance the device was **not compromised**.

---

### ⭐ 6. Automated Attacks Were the Primary Threat  
The distribution of IPs and the repetitive logon attempts strongly indicate:

- Automated scanning bots  
- Credential-stuffing scripts  
- Non-targeted attackers  
- Mass-internet probing  

This represents the **background radiation** of the internet.

---

# 🛡 Security Improvement Opportunities

### ⭐ Strengthen RDP Access  
- Block public-facing RDP entirely  
- Use Just-In-Time (JIT) access  
- Restrict RDP by IP allowlist only  

---

### ⭐ Enforce Account Lockout  
Implement lockout or throttling to slow or stop repeated brute-force attempts.

---

### ⭐ Enable MFA Wherever Possible  
MFA stops credential-based attacks even if passwords are guessed.

---

### ⭐ Improve Alerting  
Create alerts for:

- Multiple failed logons  
- Authentication from unusual geographic regions  
- Successful logon after multiple failures  
- RDP authentication from non-approved IPs  

---

### ⭐ Harden Device & Network Configuration  
- Review NSG/firewall rules regularly  
- Disable RDP unless absolutely necessary  
- Consider moving to VPN-based admin access  

---

# 🎯 Final Reflection

This threat hunt demonstrated:

- Real-world attack behavior  
- Your ability to collect and analyze log data  
- Proper use of KQL for SOC investigation  
- Correlation of attacker activity  
- Identification of missed controls  
- Professional reporting and documentation  

Most importantly:

### ⭐ The system was attacked, but **not** breached 


