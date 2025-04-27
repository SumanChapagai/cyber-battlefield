# 🧹 Log Manipulation Detection
Scenario:
An attacker attempted to cover their tracks by clearing Windows Event Logs using:

```powershell
wevtutil cl System
```

## Detection Steps:

1. Opened Splunk Search App.

2. Ran the following query to visualize Sysmon events over time:
```
index=sysmon earliest=-30m latest=now
| timechart span=2m count by host
```

3. Observed a sharp drop in log volume:

- From hundreds of events per bucket ➔ suddenly 10–20 or 0 events.
![Screenshot 2025-04-26 125825](https://github.com/user-attachments/assets/3ebd3eb9-a7e2-4d50-99c5-4961cecba7ea)

- This indicates a manual clearing of logs.

4. Verified by correlating EventCode 1102 (Windows Security log clear event).

## 📊 Bonus: Splunk Panel — Detect Log Clearing

![Screenshot 2025-04-27 121900](https://github.com/user-attachments/assets/6b532f4c-1870-4579-965b-494767456baa)

```
<panel>
  <title>🧹 Log Volume Over Time (Detect Log Clearing)</title>
  <chart>
    <search>
      <query>index=sysmon earliest=-30m latest=now | timechart span=2m count by host</query>
      <earliest>-30m</earliest>
      <latest>now</latest>
    </search>
    <option name="charting.chart">line</option>
    <option name="charting.legend.placement">right</option>
    <option name="height">300</option>
  </chart>
</panel>
```
