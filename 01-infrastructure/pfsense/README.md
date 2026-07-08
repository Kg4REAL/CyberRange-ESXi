# 🎛️ Network Architecture & pfSense Firewall Configuration

This repository section covers the network engineering, VLAN segmentation, and firewall access control lists (ACLs) implemented within the pfSense security gateway to strictly isolate offensive, defensive, and corporate target zones.

## 📐 Network Layout & Interface Mapping
The core network is segmented into four distinct security zones managed by a virtualized **pfSense 2.7.0-RELEASE** appliance. Each zone maps to a dedicated virtual interface via 802.1Q VLAN tagging on the ESXi virtual switches.

* **WAN Interface**: `192.168.40.150` (Gateway to external update repositories / simulation mirror)
* **LAN (VLAN 10 - RedTeam)**: `10.10.10.254/24` (Dedicated zone for offensive security testing tools, e.g., Kali Linux)
* **OPT1 (VLAN 20 - Targets)**: `10.10.20.254/24` (Corporate Active Directory & vulnerable services zone hosting Windows Server 2022 and Windows 7)
* **OPT2 (VLAN 30 - BlueTeam/SOC)**: `10.10.30.254/24` (Central security monitoring zone running the Wazuh SIEM stack)

---

## 📊 VLAN Segmentation Matrix

| VLAN Name | Tag ID | Subnet | Primary Role / Assets |
| :--- | :---: | :--- | :--- |
| **VLAN10-RedTeam** | `10` | `10.10.10.0/24` | Kali Linux attacker node & simulation toolsets. |
| **VLAN20-Targets** | `20` | `10.10.20.0/24` | Active Directory Domain Controller (Win Server 2022), Web Application (bWAPP on IIS), and Domain workstation (Win7). |
| **VLAN30-BlueTeam** | `30` | `10.10.30.0/24` | Wazuh SIEM/XDR Manager instance running via Docker Compose. |

---

## 🧱 Firewall Access Control Lists (ACLs)

The firewall rule design operates on a **Default-Deny** paradigm, allowing only explicit interaction required for simulation scenarios while prohibiting lateral movement or unauthorized access into the management/SOC architecture.

### Current Rules Implementation

| Interface | Source | Destination | Protocol / Port | Action | Description / Technical Justification |
| :--- | :--- | :--- | :---: | :---: | :--- |
| **LAN** | `LAN net` | `OPT2 net` | `Any` | **🛑 Block** | Prevents the attacker zone (Kali) from tampering with or scanning the SIEM infrastructure (Wazuh). |
| **LAN** | `LAN net` | `Any` | `Any` | **🟢 Allow** | Enables the Red Team node to target the internal network assets and fetch public exploits. |
| **OPT1** | `OPT1 net` | `Any` | `Any` | **🟢 Allow** | Legacy ruleset enabling targets to communicate globally and respond to multi-vector simulation queries. |
| **OPT2** | `OPT2 net` | `Any` | `Any` | **🟢 Allow** | Enables the SOC instance to reach out for package updates and maintain external threat feed syncing. |

---

## 🛠️ Security Recommendations & Hardening Plan (Blue Team Perspective)

To match enterprise security best practices and simulate a strict **Zero Trust Architecture**, the following adjustments are recommended for the next phase of lab maturation:

### 1. Tightening the Corporate Zone (OPT1 - Targets)
Allowing the vulnerable Windows environment `any` destination introduces risks of uncontrolled reverse shells over unmonitored ports. 
* **Hardening Rule**: Restrict `OPT1 net` egress traffic. Outbound access should only be allowed to `OPT2 net` on specific monitoring ports (**Wazuh Agent Ports: TCP 1514 / 1515**), and to the WAN exclusively for authorized web updates (HTTP/HTTPS).

### 2. Restricting the SIEM/SOC Zone (OPT2 - BlueTeam)
The Wazuh instance does not need to initiate connections into the RedTeam network.
* **Hardening Rule**: Change `OPT2 net -> any` to explicit stateful destinations. It should strictly allow outbound connections to the WAN for updates and inbound connections from `OPT1 net` and `LAN net` to ingest event logs.

### 3. Active Directory Inter-VLAN Considerations
Since the Windows Client (Win7) and Windows Server are inside the same subnet (`OPT1`), their Active Directory replication, Kerberos, and DNS traffic do not hit the firewall, maintaining local operational stability while enforcing isolation against external subnet sniffing.
[pfsense](../../06-reports/screenshots/pfsense_dashboard.png)
