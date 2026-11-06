# 03 - Kali Linux Attack Node Setup & Configuration

## 1. Overview

This document describes the deployment and configuration of the Kali Linux virtual machine, which serves as the attack simulation node within the SOC Home Lab environment.

The node is used to conduct controlled security testing, simulate attacker activities, and generate security events against designated lab targets. These activities provide an opportunity to evaluate the detection and monitoring capabilities of the Wazuh SIEM platform.

The Kali Linux machine operates within the isolated Host-Only network, allowing controlled communication with the Windows 11 endpoint and the central Wazuh Manager.

### Key Implementation Components

- Deployment of Kali Linux as the dedicated attack simulation machine.
- Configuration of Host-Only network connectivity.
- Verification of communication with the Wazuh Manager and Windows 11 endpoint.
- Availability of penetration testing and reconnaissance tools.
- Preparation of the environment for controlled attack simulations and detection testing.

---

## 2. Node Infrastructure Specifications

### 2.1 System Information

| Component | Configuration |
|---|---|
| Operating System | Kali Linux 2024.x |
| Virtualization Platform | VMware Workstation |
| Node Role | Attack Simulation / Security Testing |
| Network Interface | Host-Only Adapter |
| Host-Only IP Address | 192.168.56.x |

### 2.2 Security Testing Tools

The following tools are available for conducting controlled security assessments and generating attack-related network activity.

| Tool | Purpose |
|---|---|
| Nmap | Network discovery, host identification, and port scanning. |
| Hydra | Controlled authentication testing. |
| Metasploit Framework | Vulnerability assessment and exploitation testing in the isolated lab. |
| cURL | HTTP requests and web service interaction. |
| Gobuster | Directory and resource enumeration on web servers. |

---

## 3. Network Configuration and Connectivity Verification

### 3.1 Network Configuration

Kali Linux was connected to the Host-Only network to establish communication with the other virtual machines within the SOC lab environment.

This configuration allows the attack simulation node to interact with designated lab targets without requiring direct exposure to external networks.

### 3.2 Connectivity Testing

Network connectivity was verified by sending ICMP echo requests to the Wazuh Manager and Windows 11 endpoint.

The following commands were executed in the Kali Linux terminal:

```bash
# Verify connectivity to the Wazuh Manager
ping -c 3 192.168.56.102

# Verify connectivity to the Windows 11 endpoint
ping -c 3 192.168.56.105
```

**Expected Results:**

- Successful replies from the Wazuh Manager confirm network reachability within the SOC monitoring infrastructure.
- Successful replies from the Windows 11 endpoint confirm communication between the attack simulation node and the monitored workstation.

These tests establish basic network connectivity before conducting security testing activities.

---

## 4. SOC Agent Integration

### 4.1 Wazuh Agent Deployment

The Wazuh Agent was deployed on the Kali Linux machine to enable endpoint monitoring and centralized security event reporting.

The agent facilitates the collection of relevant system logs and forwards supported security events to the central Wazuh Manager.

### 4.2 Agent Status Verification

The Wazuh Agent service was checked to confirm that it was running and communicating with the Wazuh Manager.

The agent's registration and connectivity status were verified through the Wazuh Dashboard.

---

## 5. Lab Readiness and Verification

Following deployment and network configuration, the Kali Linux attack node was checked to ensure that it was ready for controlled security testing.

### Deployment Checklist

- [x] Kali Linux virtual machine successfully deployed.
- [x] Host-Only network adapter configured.
- [x] Network connectivity to the Wazuh Manager verified.
- [x] Network connectivity to the Windows 11 endpoint verified.
- [x] Security testing tools installed and available.
- [x] Wazuh Agent configured and reporting to the Wazuh Manager.

---

## 6. Outcome

The Kali Linux virtual machine was established as the dedicated attack simulation node within the SOC Home Lab environment.

Its integration into the isolated lab network provides a controlled environment for performing reconnaissance, network scanning, and other security testing activities against designated targets.

The generated activity can subsequently be investigated through the Wazuh monitoring infrastructure to assess endpoint visibility, identify security events, and evaluate detection capabilities.

This node forms an essential component of the practical SOC testing environment, supporting attack simulation, security event analysis, and detection validation.

---

## 7. Screenshots and Evidence

The following screenshots provide evidence of the deployment and configuration process:

1. Kali Linux virtual machine running in VMware Workstation.
2. Network interface configuration and assigned IP address.
3. Successful connectivity test to the Wazuh Manager.
4. Successful connectivity test to the Windows 11 endpoint.
5. Installed security testing tools.
6. Wazuh Agent service status.
7. Kali Linux endpoint registration in the Wazuh Dashboard.
