# \# Findings — Mini-SOC on Windows 11 Home

# 

# \## Summary

# One-host SOC exercise using Sysmon with SwiftOnSecurity baseline config on Windows 11 Home.  

# Simulated benign activity:

# \- Local user creation

# \- Admin group modification

# \- Scheduled task creation

# \- Certutil file download

# 

# ---

# 

# \## Finding 1 — Ingress Tool Transfer

# \- \*\*MITRE Technique:\*\* \[T1105 — Ingress Tool Transfer](https://attack.mitre.org/techniques/T1105/)  

# \- \*\*Event ID:\*\* 1 (Sysmon - Process Create)  

# \- \*\*Executable:\*\* certutil.exe  

# \- \*\*CommandLine:\*\* `certutil -urlcache -f https://example.com C:\\MiniSOC\\logs\\example.txt`  

# \- \*\*Evidence:\*\* \[screenshots/certutilexe.png](screenshots/certutilexe.png)  

# \- \*\*Notes:\*\* Certutil is a legitimate Windows tool often abused to download files. Here it was simulated safely.

# 

# ---

# 

# \## Finding 2 — Persistence via Scheduled Task

# \- \*\*MITRE Technique:\*\* \[T1053.005 — Scheduled Task/Job: Scheduled Task](https://attack.mitre.org/techniques/T1053/005/)  

# \- \*\*Event ID:\*\* 1 (Sysmon - Process Create)  

# \- \*\*Executable:\*\* schtasks.exe  

# \- \*\*CommandLine:\*\* `schtasks /create /sc onlogon /tn "DemoNotepad" /tr "notepad.exe"`  

# \- \*\*Evidence:\*\* \[screenshots/schtasks.png](screenshots/schtasks.png)  

# \- \*\*Notes:\*\* A scheduled task was created to launch Notepad at logon. Attackers often use this method for persistence.

# 

# ---

# 

# \## Finding 3 — Account Manipulation

# \- \*\*MITRE Technique:\*\* \[T1136 — Create Account](https://attack.mitre.org/techniques/T1136/)  

# \- \*\*Event ID:\*\* 1 (Sysmon - Process Create)  

# \- \*\*Executable:\*\* net1.exe  

# \- \*\*CommandLine:\*\* `net1 user labuser Passw0rd! /add`  

# \- \*\*Additional Evidence:\*\* User added to Administrators group  

# \- \*\*Evidence:\*\* \[screenshots/net1exe.png](screenshots/net1exe.png)  

# \- \*\*Notes:\*\* The demo created a local account `labuser` and added it to Administrators. This shows how attackers can persist or escalate privileges.

# 

# ---

# 

# \## Recommendations

# \- Remove any unauthorized admin accounts.  

# \- Review scheduled tasks for unknown or suspicious entries.  

# \- Monitor Sysmon Event IDs 1 and 13 for LOLBIN activity such as certutil usage.



