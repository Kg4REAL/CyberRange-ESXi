# Detection — Network Reconnaissance

## Wazuh Alerts to Monitor

| Rule ID | Description | Level |
|---|---|---|
| 5710 | Multiple connection attempts | 8 |
| 5711 | Port scan detected | 10 |
| 18151 | Nmap scan detected | 12 |

## IOCs
- Multiple SYN packets from 10.10.10.10
- Requests to ports 53, 88, 135, 389, 445 in short time
- LDAP enumeration requests

## Response
1. Check Wazuh alerts dashboard
2. Block source IP on pfSense
3. Document in incident report
