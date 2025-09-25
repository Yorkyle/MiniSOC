# Mini-SOC on Windows 11 Home

Built and executed a one-host SOC proof-of-concept on Windows 11 Home using Sysmon (Sysinternals) and community config.
Simulated and detected ingress (LOLBIN download), persistence (scheduled task), and account manipulation. 
Investigated via Event Viewer, exported EVTX evidence, and mapped findings to MITRE ATT&CK (T1105, T1053.005, T1136). 
Full write-up and artifacts included.

## Repo structure
- custom-views/ : XML filters for Event Viewer
- logs/ : exported .evtx and hashes
- screenshots/ : screenshots of findings
- artifacts/ : task XMLs, playbooks, other exports
- README.md : project overview
- FINDINGS.md : investigation write-up
