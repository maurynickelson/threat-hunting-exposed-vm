# ⭐ Response (Recommended Remediation Steps)

## **Objective**
Outline the remediation steps that should be taken to secure the exposed VM, prevent brute-force attacks, and strengthen authentication security.

---

# 🛡️ 1. Harden the Network Security Group (NSG)
### **Recommended Action**
Restrict RDP access to trusted IP addresses only and block all public inbound traffic to the VM.

### **Purpose**
- Close the exposure that allowed brute-force attempts  
- Reduce attack surface  
- Enforce least-privilege access  

---

# 🔐 2. Implement Account Lockout Policies
### **Recommended Action**
Enable account lockout thresholds so repeated failed login attempts result in temporary lockouts.

### **Purpose**
- Interrupt brute-force password guessing  
- Slow down attackers  
- Improve overall authentication security  

---

# 🔑 3. Enable Multi-Factor Authentication (MFA)
### **Recommended Action**
Enable MFA to ensure that even if credentials are compromised, attackers cannot log in.

### **Purpose**
- Adds a second layer of protection  
- Prevents unauthorized access  
- Helps secure privileged accounts  

---

# ⭐ Summary
These remediation steps, when implemented, will significantly reduce exposure to brute-force attacks and improve the security posture of the VM and its associated accounts.
