# 🔍 Sysmon Log Visualization in Splunk

## 🎯 Objective
Enable and visualize Windows network activity (EventCode 3) from Sysmon in Splunk to identify potential command-and-control (C2) or suspicious outbound behavior.

---

## 🛠️ Environment

| Role     | OS           | IP Address     |
|----------|--------------|----------------|
| Defender | Ubuntu (Splunk) | `192.168.0.10` |
| Victim   | Windows 10   | `192.168.0.12` |

---

## 📦 Log Forwarding Setup

### 🧩 Sysmon Configuration
- Reinstalled Sysmon after failed event logging attempts
- Confirmed network monitoring enabled via:
  ```powershell
  Sysmon.exe -c
  ```
### 📤 Universal Forwarder Configuration (inputs.conf)
[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = 0
index = sysmon
renderXml = true
sourcetype = XmlWinEventLog

### 📬 Forwarding Destination (outputs.conf)
[tcpout:default-autolb-group]
server = 192.168.0.10:9997

### ❗ Issue: Missing Fields in Splunk
EventCode 3 logs were visible, but fields like Image, User, DestinationIp, and DestinationPort were not parsed due to XML structure.

## ✅ Fix: Use spath for On-the-Fly Field Extraction
```
index=sysmon EventCode=3
| spath input=_raw
| eval Image=spath(_raw, "Event.EventData.Data{5}")
| eval User=spath(_raw, "Event.EventData.Data{6}")
| eval DestinationIp=spath(_raw, "Event.EventData.Data{15}")
| eval DestinationPort=spath(_raw, "Event.EventData.Data{17}")
| table _time, Image, User, DestinationIp, DestinationPort
```
### 📊 Dashboard Panel
![Screenshot 2025-04-25 110535](https://github.com/user-attachments/assets/a7151043-427a-4761-8b6d-9ccc4b7bd6bf)


### 🧠 Learnings
- Splunk may not parse XML fields automatically for custom Sysmon logs
- spath is reliable for direct XML extraction
- Dashboard-ready data can be achieved without props.conf or regex configs
