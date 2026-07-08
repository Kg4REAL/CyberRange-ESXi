# Detection — Persistence

## Windows Event IDs
| Event ID | Description |
|---|---|
| 4698 | Scheduled task created |
| 4702 | Scheduled task updated |
| 13 | Registry value set (Sysmon) |

## IOCs
- New registry key in Run/RunOnce
- Scheduled task created by john.doe
- Executable in C:\Windows\Temp

## Response
1. Remove malicious registry key
2. Delete unauthorized scheduled task
3. Scan system with AV
4. Reset compromised credentials
