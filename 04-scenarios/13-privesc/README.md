# Scenario 13 — Privilege Escalation

![MITRE](https://img.shields.io/badge/MITRE-T1068%20%7C%20T1078-red)
![Status](https://img.shields.io/badge/Status-Planned-yellow)
![Tool](https://img.shields.io/badge/Tool-WinPEAS%20%7C%20LinPEAS-orange)

## MITRE ATT&CK
- T1068 — Exploitation for Privilege Escalation
- T1078 — Valid Accounts

## Tools
- WinPEAS
- LinPEAS

## Windows Privesc

### Run WinPEAS
```bash
# Upload depuis Kali
crackmapexec smb 10.10.20.10 -u john.doe -p Password123 --put-file winpeas.exe C:\\Windows\\Temp\\winpeas.exe
```

### Check misconfigured services
```bash
accesschk.exe -uwcqv "john.doe" *
```

## Detection
See: 05-detection/wazuh-rules/04-privesc-detection.md
