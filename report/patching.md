# 🛡️ System Patching & Hardening (Victim Machine)

## 📍 Overview
After detecting the presence of malicious activities (reverse shell via `shell.exe`), the following actions were taken to **remediate** and **harden** the Windows 10 victim machine.

---

## 🛠️ Actions Taken

| Step | Description | Status |
|:---|:---|:---|
| 1 | **Removed malicious binaries** (e.g., `shell.exe`) | ✅ Completed |
| 2 | **Re-enabled Windows Defender** | ✅ Completed |
| 3 | **Applied latest Windows Updates manually** | ✅ Completed |
| 4 | **Reviewed active network connections** | ✅ Completed |
| 5 | **Verified Sysmon service is active and logging** | ✅ Completed |
| 6 | **Reset firewall rules to default secure settings** | ✅ Completed |

---

## 🔥 Key Commands Used

```powershell

# Re-enable Windows Defender
Set-MpPreference -DisableRealtimeMonitoring $false

# Verify Sysmon status
sc query sysmon
```

## 📈 Post-Patch Verification
No active reverse shells detected.

All suspicious files cleaned.

Windows Firewall operational.

Sysmon active for future incident detection.

Security Event Logs reviewed for additional compromise attempts.

## 📚 Lessons Learned
Regular patching and monitoring are critical.
Proactive Sysmon + Splunk setup helped early detect suspicious behavior.

---
