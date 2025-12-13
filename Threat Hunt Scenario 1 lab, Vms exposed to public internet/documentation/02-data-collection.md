# ⭐ 02 — Data Collection

## 📝 Objective
Gather all necessary log sources and validate that the exposed VM was truly internet-facing during the window of interest.

---

# 🔍 Step 1 — Confirm the Device Was Internet-Facing

### KQL Query
````kql
DeviceInfo
| where DeviceName == "notengocyberlab"
| where IsInternetFacing == true
| order by Timestamp desc

![Internet Facing Status](/images/analysis/device-internet-facing.png)
