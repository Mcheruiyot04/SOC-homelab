# Enterprise SOC Lab Architecture & Design

## 1. Executive Summary

This document details the architectural layout, network topology, and security monitoring design of an isolated home Security Operations Center (SOC) lab.

The environment simulates an enterprise endpoint-to-SIEM monitoring pipeline, capturing endpoint telemetry and forwarding security events to a centralized monitoring platform for threat detection, alert analysis, and incident investigation.

The lab consists of three virtual machines:

- **Ubuntu Linux:** Hosts the Wazuh SIEM Manager.
- **Windows 11:** Acts as the monitored endpoint, generating security events through Sysmon.
- **Kali Linux:** Acts as the attacker machine for controlled security testing and attack simulations.

The primary objective is to understand how endpoint activities are monitored, how security events are collected, and how suspicious activities are detected and investigated using Wazuh.

## 2. High-Level Architecture Diagram

The following diagram illustrates the relationship between the virtual machines and the security monitoring pipeline.
```mermaid
 H["HOST SYSTEM<br/>Oracle VirtualBox"]

    subgraph N["Isolated Virtual Network — Host-Only Network"]
        K["Kali Linux<br/>Attacker Node<br/>IP: [Kali IP]"]
        W["Windows 11<br/>Monitored Endpoint<br/>IP: [Windows IP]<br/>Sysmon + Wazuh Agent"]
        U["Ubuntu Linux<br/>Wazuh SIEM Manager<br/>IP: [Ubuntu IP]"]
    end

    K -->|"Controlled attack simulations"| W
    W -->|"Sysmon generates telemetry"| A["Wazuh Agent"]
    A -->|"Forwards security events"| U
    U -->|"Analyzes events and generates alerts"| D["Wazuh Dashboard"]

    H -.->|"Virtualization platform"| N
```


## 3. Lab Components and Roles

| Component | Operating System / Tool | Primary Role |
|---|---|---|
| SIEM Server | Ubuntu Linux + Wazuh | Centralized security monitoring and alert management |
| Monitored Endpoint | Windows 11 | Generates endpoint security events |
| Telemetry Collection | Sysmon | Captures detailed Windows system activity |
| Endpoint Agent | Wazuh Agent | Forwards collected events to the Wazuh Manager |
| Attacker Machine | Kali Linux | Performs controlled attack simulations |
| Virtualization | Oracle VirtualBox | Hosts and isolates the virtual machines |

## 4. Network Topology

The lab uses a virtual network to facilitate communication between the virtual machines.

**Network Configuration:**

- Network Type: Host-Only Network 
- Network Subnet: [Actual subnet]
- Network Mask: [Actual subnet mask]

**Virtual Machine IP Addresses:**

| Virtual Machine | Assigned IP Address |
|---|---|
| Ubuntu Wazuh Manager | [192.168.56.102] |
| Windows 11 Endpoint | [192.168.56.105] |
| Kali Linux Attacker | [192.168.56.104] |

The virtual machines communicate over the configured lab network. Kali Linux is used to generate controlled activity against the Windows endpoint, while the Windows endpoint forwards relevant security telemetry to the Wazuh Manager.

## 5. Security Monitoring and Data Flow

The SOC lab implements an endpoint-to-SIEM monitoring pipeline.

The data flow follows these steps:

1. **Activity Generation:** Kali Linux performs controlled network scans and security testing against the Windows 11 endpoint.

2. **Event Generation:** Windows 11 generates system and security events during normal operations and simulated activities.

3. **Telemetry Collection:** Sysmon captures detailed endpoint activities, including process creation, network connections, and other system events.

4. **Event Forwarding:** The Wazuh Agent collects the configured Sysmon event logs and forwards them to the Wazuh Manager.

5. **Event Analysis:** Wazuh processes incoming events and applies its configured rules to identify potentially suspicious activities.

6. **Alert Generation:** Security alerts are displayed on the Wazuh Dashboard for monitoring and investigation.

## 6. Security Objectives

The architecture is designed to achieve the following objectives:

- Establish centralized security event monitoring.
- Monitor Windows endpoint activities.
- Collect and analyze Sysmon telemetry.
- Detect suspicious network and system activities.
- Practice security alert triage and investigation.
- Understand the relationship between endpoint telemetry and SIEM platforms.

## 7. Conclusion

This architecture provides a controlled environment for understanding fundamental Security Operations Center (SOC) workflows.

By integrating Kali Linux, Windows 11, Sysmon, and Wazuh, the lab provides practical exposure to endpoint monitoring, security event collection, threat detection, and incident investigation.

The environment will be progressively expanded with additional detection rules, attack simulations, and investigation exercises as part of the ongoing cybersecurity learning process.
