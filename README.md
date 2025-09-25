# Mini-SOC on Windows 11 Home

A one-host SOC proof-of-concept performed on a Windows 11 Home machine using Sysmon (Sysinternals) and Event Viewer.

## Summary
This project demonstrates a lightweight detection workflow:
- Installed Sysmon with a community config to capture process and registry telemetry.
- Simulated benign activity: local account creation, scheduled task persistence, and certutil registry activity (LOLBIN observation).
- Investigated with Event Viewer, captured screenshots, and documented findings.
- Mapped observed activity to MITRE ATT&CK techniques.

## Tools
- Sysmon (Sysinternals)
- Windows Event Viewer
- Built-in Windows tools used for simulation: `net`, `schtasks`, `certutil`

## Findings (high level)
1. **Persistence** — Scheduled task created to run Notepad  
   - MITRE: [T1053.005 — Scheduled Task/Job: Scheduled Task](https://attack.mitre.org/techniques/T1053/005/)  
   - Evidence: `screenshots/schtasks.png`

2. **Account creation** — Local user `labuser` created (demo)  
   - MITRE: [T1136 — Create Account](https://attack.mitre.org/techniques/T1136/)  
   - Evidence: `screenshots/net1exe.png`

3. **LOLBIN / registry activity** — `certutil` observed in registry modification  
   - MITRE: [T1105 — Ingress Tool Transfer](https://attack.mitre.org/techniques/T1105/)  
   - Evidence: `screenshots/certutilexe.png`

## What’s included
- `FINDINGS.md` — investigation notes with MITRE mapping.
- `screenshots/` — redacted images of Event Viewer showing key events.
- `artifacts/` — optional exported task XML and analyst playbook.

## Notes & safety
- All activity was performed locally in a controlled lab environment.  
- Commands executed were benign and used only for demonstration.  
- Screenshots have been redacted to remove system and user-identifying information.  
- Raw EVTX logs are excluded to protect privacy.

## How to reproduce (brief)
1. Install Sysmon with a baseline config.
2. Run the following commands in an elevated command prompt:
   - `net user labuser Passw0rd! /add`
   - `schtasks /create /sc onlogon /tn "DemoNotepad" /tr "notepad.exe"`
   - `certutil -urlcache -f https://example.com C:\MiniSOC\logs\example.txt`
3. Inspect Sysmon Operational log in Event Viewer (filter Event ID 1 & 13).
