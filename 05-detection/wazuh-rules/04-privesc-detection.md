# Detection — Privilege Escalation

## Windows Event IDs
| Event ID | Description |
|---|---|
| 4672 | Special privileges assigned |
| 4673 | Privileged service called |
| 7045 | New service installed |

## IOCs
- WinPEAS execution in C:\Windows\Temp
- New service created by non-admin user
- Unusual process with SYSTEM privileges

## Response
1. Kill malicious process
2. Remove unauthorized service
3. Review privilege assignments
