# Log Manipulation (Sysmon Event Deletion)

## 🎯 Objective
Simulate attacker behavior by clearing Windows Event Logs to cover tracks.

## 🖥️ Target System
- Victim Machine (Windows 10)
- Static IP: `192.168.0.12`

## ⚙️ Method
Used `wevtutil` command in an elevated PowerShell session to clear Sysmon logs:

```powershell
wevtutil cl "Microsoft-Windows-Sysmon/Operational"
```

## 🕵️‍♂️ Observation
- Sysmon logs were cleared.
- Verified in Splunk: no events were found for index=sysmon during the time gap.
- Verified in Event Viewer: Sysmon log channel was empty immediately after command execution.

## 📸 Screenshots
- Before clearing
![Screenshot 2025-04-26 125143](https://github.com/user-attachments/assets/ba90d4d1-b89c-47bf-855e-a44c7fee0325)
- After clearing
![Screenshot 2025-04-26 125150](https://github.com/user-attachments/assets/c5cc899d-a2a4-4d3d-ad3a-deb301b6180a)

## ⚡ Notes
- Clearing logs is a common attacker technique (T1070.001 - MITRE ATT&CK).
- Monitoring log deletion activities is critical for defense.
