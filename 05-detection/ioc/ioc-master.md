# Indicators of Compromise (IOCs)

## Network IOCs
| IOC | Type | Scenario | Description |
|---|---|---|---|
| 10.10.10.10 | IP | All | Kali attacker IP |
| Multiple SYN | Traffic | Recon | Port scan |
| LLMNR response | Traffic | Pivoting | Responder active |

## Windows Event IOCs
| Event ID | Scenario | Severity |
|---|---|---|
| 4769 (multiple) | Kerberoasting | High |
| 4768 (pre-auth fail) | ASREPRoasting | High |
| 4624 type 3 NTLM | Pass-the-Hash | High |
| 4698 | Persistence | Medium |
| 7045 | Privesc | High |
| 13 (Sysmon) | Persistence | Medium |

## File IOCs
| File | Location | Scenario |
|---|---|---|
| winpeas.exe | C:\Windows\Temp | Privesc |
| shell.exe | C:\Windows\Temp | Persistence |
| passwords.txt | C:\Share | Recon |
