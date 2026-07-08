# Detection — LLMNR Poisoning & Pivoting

## Windows Event IDs
| Event ID | Description |
|---|---|
| 4648 | Logon with explicit credentials |
| 5379 | Credential Manager accessed |

## IOCs
- LLMNR/NBT-NS responses from unexpected host
- NTLMv2 authentication from 10.10.10.10
- Responder traffic on network

## Response
1. Disable LLMNR via GPO
2. Reset credentials of affected users
3. Enable SMB signing
