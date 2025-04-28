# 🛡️ Cyber Battlefield - Incident Report

## 1. Executive Summary
- **Date**: April 2025
- **Environment**: Windows 10 Enterprise Evaluation (Victim), Ubuntu (Defender/Splunk), Kali Linux (Attacker)
- **Summary**: A simulated attacker compromised the victim system, established persistence, and exfiltrated limited data. Defensive measures including detection, analysis, and remediation were successfully executed.

---

## 2. Attack Timeline
| Time (EDT) | Event |
|:---|:---|
| Day 2 | Initial Nmap scanning from Kali to Windows |
| Day 3 | Logging setup using Sysmon and Splunk |
| Day 4 | Reverse shell established, persistence implanted |
| Day 5 | Logs stealthily cleared (partial success) |
| Day 6 | Full C2 session with payloads, endpoint compromise |
| Day 7 | Patching, clean-up, and report generation |

---

## 3. Key Findings
- **Vulnerability Exploited**: Lack of endpoint protection and exposed services.
- **Persistence Achieved**: Via Windows Registry Run key with malicious PowerShell script.
- **Detection**:
  - Sysmon EventID 1: Process Creation
  - Sysmon EventID 3: Network Connections (port 4444)
- **Stealth Techniques**:
  - Event clearing using `wevtutil`
  - Manual hiding of shell artifacts.

---

## 4. Remediation Actions
| Action | Status |
|:---|:---|
| Identified suspicious events (shell.exe, EventCode 1, 3) | ✅ |
| Disabled and removed malicious persistence | ✅ |
| Cleared rogue files (payload.ps1, shell.exe) | ✅ |
| Applied Windows Updates manually | ✅ |
| Re-enabled Windows Defender (planned) | 🚧 (for next phase) |

---

## 5. Recommendations
- Harden host firewall to limit lateral movement.
- Implement application whitelisting.
- Set up centralized logging with alerting (Splunk SIEM tuning).
- Regularly audit Windows services and autoruns.
- Enforce strict user privilege separation (no admin rights).

---

# 📅 Status: **Completed**
