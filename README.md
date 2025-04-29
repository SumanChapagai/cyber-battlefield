# 🛡️ Cyber Battlefield

A beginner-friendly cybersecurity lab simulating real-world attack and defense scenarios. This project is designed to showcase red team vs blue team techniques with clear documentation, monitoring, and detection.

## 🎯 Project Goals
- Learn and simulate attacker vs. defender cybersecurity techniques
- Use tools like Nmap, Metasploit, Splunk, and Wireshark
- Monitor attacks, detect them, and build incident response playbooks
- Document everything for GitHub as a public showcase

## 🖥️ Virtual Machines
| Role      | OS           | IP Address      | Purpose |
|-----------|--------------|----------------|---------|
| Attacker  | Kali Linux   | 192.168.0.11    | Launches attacks, scans, exploits |
| Defender  | Ubuntu       | 192.168.0.10    | Detects and responds with Splunk/Wireshark |
| Victim    | Windows 10   | 192.168.0.12    | Hosts vulnerable services (DVWA, RDP, SMB) |

## 📅 Timeline
| Day | Goal |
|-----|------|
| 1 | VM Setup + Networking |
| 2 | Scanning & Initial Exploit |
| 3 | Logging & Detection |
| 4 | C2 Channel + Persistence |
| 5 | Stealth + Log Manipulation |
| 6 | Detection + Response |
| 7 | Patch & Final Report |

## 📂 Project Structure

```cyber-battlefield/
├── README.md  
├── attack_scenarios/
│   ├── c2_payload.md  
│   ├── log_manipulation.md  
│   ├── persistence.md  
│   ├── reverse_shell_exe.md  
│   └── scan_and_exploit.md  
├── automation/  
│   └── packet_capture_script.py  
├── defender_setup/  
│   └── splunk_setup.md  
├── defense/  
│   ├── splunk_dashboard.md
│   ├── splunk_logs.md  
│   └── wireshark_analysis.md  
├── report/
│   ├── incident_report.md  
│   └── patching.md  
├── setup/  
│   └── VM_setup.md  
```

## 🧰 Tools Used
- Nmap, Metasploit, Netcat, Python (Attacker)
- Splunk, Wireshark, Sysmon (Defender)
- DVWA, SMB, RDP (Victim)

---
