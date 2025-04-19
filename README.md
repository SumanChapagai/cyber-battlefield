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
cyber-battlefield/
├── README.md
├── setup/
│   └── VM_setup.md
├── attack_scenarios/
│   ├── scan_and_exploit.md
│   └── c2_payload.md
├── defense/
│   ├── splunk_logs.md
│   └── wireshark_analysis.md
├── automation/
│   └── packet_capture_script.py
├── report/
│   └── incident_report.md



## 🧰 Tools Used
- Nmap, Metasploit, Netcat, Python (Attacker)
- Splunk, Wireshark, Sysmon (Defender)
- DVWA, SMB, RDP (Victim)

---
