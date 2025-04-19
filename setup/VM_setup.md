# VM Setup Documentation

## 🖥️ Virtual Machines Used

### 1. Kali Linux (Attacker)
- OS: Kali GNU/Linux
- Version: Latest ISO with GNOME
- Resources: 2 CPUs, 2 GB RAM
- Network Adapters:
  - Adapter 1: NAT
  - Adapter 2: Host-Only (192.168.0.11)
- Tools Installed: Nmap, Metasploit, Wireshark, Netcat, Python

### 2. Ubuntu (Defender)
- OS: Ubuntu Desktop
- Resources: 2 CPUs, 4 GB RAM
- Network Adapters:
  - Adapter 1: NAT
  - Adapter 2: Host-Only (192.168.0.10)
- Tools: Splunk (to be installed), Wireshark, Sysmon (planned), Python

### 3. Windows 10 (Victim)
- OS: Windows 10 Enterprise Eval
- Resources: 2 CPUs, 4 GB RAM
- Network Adapters:
  - Adapter 1: Host-Only (192.168.0.12)
- Tools: XAMPP, DVWA, Sysmon (planned)
- Vulnerabilities: Apache, MySQL, DVWA (security set to low), RDP enabled

## 🌐 Networking Configuration

- Host-Only network with IPv4 prefix: `192.168.0.1/24`
- All VMs assigned static IPs
- Verified VM-to-VM communication via `ping`
- Windows Firewall temporarily disabled for ICMP testing

---

> This setup simulates an internal network environment to perform red vs blue team activities.
> It will also help in familiarizing with the roles of a security analyst further down the line.
