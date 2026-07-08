# Detection — Web Exploitation

## Wazuh Alerts
| Rule ID | Description | Level |
|---|---|---|
| 31101 | Web attack detected | 10 |
| 31106 | SQL injection attempt | 12 |

## IOCs
- Unusual HTTP requests with SQL payloads
- Error 500 responses
- Requests from 10.10.10.10

## Response
1. Block source IP
2. Patch vulnerable application
3. Review web server logs
