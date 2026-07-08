# 🛡️ SOC Infrastructure & Wazuh SIEM/XDR Deployment

This module documents the centralization of security logs, endpoint monitoring, and the integration of the Security Operations Center (SOC) subsystem within the virtualized Cyber Range.

## 🧠 SIEM Architecture Specifications
The detection engine is centralized within an isolated security monitoring zone (`VLAN 30`), aggregating logs across distinct infrastructure layers to map attacker behaviors to the MITRE ATT&CK framework.

* **SIEM Management Host**: Ubuntu Server 24.04.4 LTS (`10.10.30.10`)
* **SIEM Core Platform**: Wazuh v4.14.5 (Single-node deployment utilizing Docker Compose)
* **Web UI Access Link**: `https://10.10.30.10`
* **Integrated Stack Components**: 
  * **Wazuh Manager**: Processes alerts, checks log signatures against rulesets, and fires alarms.
  * **Wazuh Indexer**: High-performance OpenSearch engine for parsing and storing indexing data.
  * **Wazuh Dashboard**: Visual mining portal for incident response and analytical triage.

---

## 👥 Deployed Monitoring Agents Matrix

Telemetry is ingested via endpoints running native lightweight Wazuh agent services :

| Agent ID | Endpoint Hostname | IP Address | Operating System | Operational Status | Audited Core Events |
| :---: | :--- | :--- | :--- | :---: | :--- |
| **001** | `SRV-DC-01` | `10.10.20.10` | Windows Server 2022 | ✅ Active | Active Directory Events, Sysmon telemetry, IIS Web Exploitation logs. |
| **003** | `win7-labo` | `10.10.20.51` | Windows 7 Professional | ✅ Active | User execution tracking, local process spawn behavior, host anomalies. |
| **004** | `ubuntserver` | `10.10.30.10` | Ubuntu 24.04.4 LTS | ✅ Active | Docker daemon logs, system authorization checks (`/var/log/auth.log`). |

---

## ⚙️ Network Device Integration: The pfSense Syslog Issue

### Problem Statement
While host endpoints communicate natively via the Wazuh Agent service, network security gateways running hardened architectures (such as **pfSense 2.7.0** on FreeBSD) do not support third-party agent installations. Consequently, the pfSense firewall is currently blind on the default agent overview board.

### Technical Remediation: Agentless Syslog Ingestion Pipeline

To incorporate the network visibility matrix into the SIEM dashboard, an **Agentless Remote Syslog Forwarding** configuration must be deployed :

[ pfSense Gateway ] --( Syslog UDP/514 )--> [ Wazuh Manager (Ubuntu) ]


#### Step 1: Provisioning the Wazuh Manager Configuration
The Wazuh Manager configuration file (`/var/ossec/etc/ossec.conf` inside the Docker manager container) is updated to bind an active remote syslog listener loop on port `514` :

```xml
<ossec_config>
  <remote>
    <connection>syslog</connection>
    <port>514</port>
    <protocol>udp</protocol>
    <allowed-ips>192.168.40.150</allowed-ips>
  </remote>
</ossec_config>
