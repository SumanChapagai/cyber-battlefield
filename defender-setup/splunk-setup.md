# Splunk Setup (Defender)

## 🧠 Overview
This document outlines the setup of **Splunk Enterprise** on the **Defender (Ubuntu)** machine and the configuration of **log forwarding** from the **Victim (Windows 10)** machine using **Sysmon** and the **Splunk Universal Forwarder**.

---

## 🛡️ Defender (Ubuntu VM) - Splunk Installation

### 🖥️ System Info:
- **OS**: Ubuntu
- **Role**: Defender / SIEM
- **Static IP**: `192.168.0.10`

### ✅ Steps Taken:
1. **Downloaded Splunk Enterprise** `.deb` package from [splunk.com](https://www.splunk.com).
2. **Installed Splunk**:
   ```bash
   sudo dpkg -i splunk_package_name.deb
   sudo /opt/splunk/bin/splunk start --accept-license
   sudo /opt/splunk/bin/splunk enable boot-start
3. Accessed Splunk Web UI at http://192.168.0.10:8000
4. Created necessary indexes:
	Sysmon
	windows
5. Enabled receiving on port 9997 through the Web UI:
Settings → Forwarding and Receiving → Configure Receiving

## 🎯 Defender (Ubuntu VM) - Splunk Installation
### 🖥️ System Info:
- **OS**: Windows 10
- **Role**: Victim / Endpoint
- **Static IP**: `192.168.0.12`
### 🔧 Components:
- **Sysmon** - Installed via Sysinternals Suite
- **Splunk Universal Forwarder**

### 📁 Configuration Files:

inputs.conf
```
[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = 0
index = sysmon
renderXml = true

[monitor://C:\Windows\System32\LogFiles\Firewall\pfirewall.log]
sourcetype = windows_firewall
index = test
```
outputs.conf
```
[tcpout]
defaultGroup = default-autolb-group

[tcpout:default-autolb-group]
server = 192.168.0.10:9997

[tcpout-server://192.168.0.10:9997]
```
## 🧱Index setup on Splunk
Created the following indexes to organize log types:
- sysmon → for Sysmon event logs
- test → for firewall log forwarding tests

## 🛠️Troubleshooting
### Initially encountered the following error in splunkd.log
```Could not subscribe to Windows Event Log channel 'Microsoft-Windows-Sysmon/Operational': errorCode=5```
## Solutions Applied:
### 1. Verified Sysmon installation:
- Confirmed logging was active via Event Viewer under:

  Applications and Services Logs → Microsoft → Windows → Sysmon → Operational
### 2. Modified SplunkForwarder service permissions:
- Opened services.msc
- Located SplunkForwarder
- Right-click → Properties → Log On tab
- Selected "Local System account" and enabled
	✅ "Allow service to interact with desktop"
### 3. Restarted splunkforwarder service:
```Restart-Service splunkforwarder```

✅ Final Result
Logs from the Windows 10 machine are now successfully being forwarded to Splunk on the Ubuntu defender machine and appear under the sysmon index.
