# Detection — Active Directory Attacks

## Kerberoasting
| Event ID | Description |
|---|---|
| 4769 | Kerberos service ticket requested |
| 4770 | Kerberos ticket renewed |

## ASREPRoasting
| Event ID | Description |
|---|---|
| 4768 | Kerberos pre-auth failed |

## Pass-the-Hash
| Event ID | Description |
|---|---|
| 4624 | Logon type 3 (network) |
| 4625 | Failed logon |
| 4648 | Explicit credential logon |

## IOCs
- Multiple 4769 events in short time = Kerberoasting
- 4768 with pre-auth disabled = ASREPRoasting
- Logon type 3 with NTLM = Pass-the-Hash

## Response
1. Identify source IP
2. Reset compromised account password
3. Enable Kerberos pre-auth on all accounts
4. Document timeline
