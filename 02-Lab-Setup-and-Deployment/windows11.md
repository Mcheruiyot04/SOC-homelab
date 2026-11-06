# 02 - Windows 11 Endpoint Monitoring & Sysmon Integration

## 1. Overview

This document describes the deployment and configuration of a Windows 11 endpoint within the SOC Home Lab environment. The endpoint functions as a monitored workstation, generating host-based security telemetry for centralized log collection, threat detection, and security event analysis through the Wazuh SIEM platform.

The implementation focuses on establishing endpoint visibility through Microsoft Sysmon and the Wazuh Agent, enabling the centralized monitoring and investigation of security-related activities.

### Key Implementation Components

- Deployment and configuration of Microsoft Sysmon for advanced endpoint monitoring.
- Installation and integration of the Wazuh Agent for centralized log collection.
- Configuration of dual network adapters to support network segmentation.
- Collection and forwarding of Windows Event Logs and Sysmon telemetry.
- Integration with the Wazuh Manager for centralized security monitoring.

---

## 2. Endpoint Infrastructure Specifications

### 2.1 System Information

| Component | Configuration |
|---|---|
| Operating System | Windows 11 Enterprise |
| Virtualization Platform | VMware Workstation |
| Endpoint Role | Monitored Workstation / Attack Simulation Target |
| Hostname | WIN11-Endpoint |

### 2.2 Network Configuration

The Windows 11 virtual machine uses a dual-network adapter configuration to support communication within the SOC monitoring environment while maintaining internet connectivity.

| Network Adapter | Purpose | IP Address |
|---|---|---|
| Host-Only | SOC Telemetry Network | 192.168.56.105 |
| NAT | Internet Access | Dynamically Assigned |

**Network Design and Segmentation**

The Host-Only network facilitates communication between the Windows endpoint and the Wazuh Manager (`192.168.56.102`), allowing security events and endpoint telemetry to be forwarded to the centralized monitoring server.

The NAT adapter provides internet connectivity for system updates, software downloads, and other necessary operations. During isolated security testing and attack simulations, the NAT adapter can be disabled to restrict external network access and maintain the isolation of the lab environment.

### 2.3 Security Monitoring Components

The following components were deployed to facilitate endpoint monitoring and telemetry collection:

| Component | Service / Log Source | Function |
|---|---|---|
| Wazuh Agent | `wazuhSvc` | Collects and forwards endpoint security events to the Wazuh Manager. |
| Microsoft Sysmon | `Sysmon64` | Generates detailed system activity and security telemetry. |
| Windows Event Logging | Windows Event Logs | Records operating system and security-related events. |
| Sysmon Logging | Microsoft-Windows-Sysmon/Operational | Captures detailed process, network, and system activity. |

---

## 3. Microsoft Sysmon Deployment and Configuration

### 3.1 Objective

Microsoft Sysmon was deployed to enhance endpoint visibility by capturing detailed system activity that supports security monitoring, threat hunting, incident investigation, and detection engineering.

Unlike standard Windows event logging, Sysmon provides additional visibility into activities such as process creation, network connections, file creation, and changes to system components.

### 3.2 Installation Procedure

Sysmon was installed using an elevated Command Prompt with administrative privileges.

The installation files and configuration file were first placed in the `C:\Sysmon` directory.

The following commands were executed:

```cmd
cd C:\Sysmon
Sysmon64.exe -i sysmonconfig-export.xml -accepteula
```

**Command Explanation:**

- `cd C:\Sysmon` – Navigates to the directory containing the Sysmon installation files.
- `Sysmon64.exe` – Executes the 64-bit Sysmon installation utility.
- `-i` – Installs Sysmon and applies the specified configuration file.
- `sysmonconfig-export.xml` – Specifies the Sysmon configuration used for event collection.
- `-accepteula` – Automatically accepts the Microsoft Sysinternals license agreement.

### 3.3 Sysmon Configuration Baseline

The deployment uses the **SwiftOnSecurity Sysmon Configuration**, which provides predefined filtering rules for collecting security-relevant system events while minimizing unnecessary logging.

The configuration improves endpoint visibility by monitoring activities such as:

- Process creation and execution.
- Network connection activity.
- File creation and modification events.
- Process termination.
- Registry modifications.
- Driver and image loading activities.

The configuration determines which Sysmon events are generated and made available for subsequent analysis.

### 3.4 Service Verification

Following installation, the Sysmon service was checked to confirm that it was successfully installed and running.

The following command was executed:

```cmd
sc query Sysmon64
```

The service status was reviewed to verify successful deployment.

### 3.5 Sysmon Event Log Verification

Sysmon event generation was validated through Windows Event Viewer.

**Navigation Path:**

`Event Viewer → Applications and Services Logs → Microsoft → Windows → Sysmon → Operational`

The Operational log was inspected to confirm that Sysmon was generating endpoint telemetry.

---

## 4. Wazuh Agent Deployment and Integration

*This section documents the installation, configuration, and integration of the Wazuh Agent with the central Wazuh Manager.*

### 4.1 Objective

The Wazuh Agent was deployed on the Windows 11 endpoint to facilitate the collection and forwarding of security events to the central Wazuh Manager for centralized monitoring and analysis.

### 4.2 Agent Configuration

The Wazuh Agent was configured to communicate with the Wazuh Manager using the Host-Only network interface.

| Configuration | Value |
|---|---|
| Wazuh Manager IP | 192.168.56.102 |
| Windows Endpoint IP | 192.168.56.105 |
| Agent Service | `wazuhSvc` |
| Communication Port | 1514 |

### 4.3 Log Collection Configuration

The agent was configured to collect relevant Windows security and Sysmon event logs.

The Sysmon Operational channel provides detailed endpoint telemetry that can be forwarded to the Wazuh Manager for security event analysis.

### 4.4 Agent Service Verification

The Wazuh Agent service was checked to confirm that it was running successfully.

```cmd
sc query wazuhSvc
```

The agent's connectivity and registration status were subsequently verified through the Wazuh Dashboard.

---

## 5. Endpoint Integration Verification

The final stage involved validating communication between the Windows 11 endpoint and the centralized Wazuh monitoring infrastructure.

The following checks were performed:

- Confirmed that the Sysmon service was running.
- Verified that Sysmon was generating events in Windows Event Viewer.
- Confirmed that the Wazuh Agent service was operational.
- Verified communication between the endpoint and Wazuh Manager.
- Checked the Wazuh Dashboard for endpoint registration and event visibility.

Successful integration enables the SOC environment to collect, centralize, and analyze security telemetry generated by the Windows endpoint.

---

## 6. Outcome

The Windows 11 endpoint was successfully configured as a monitored workstation within the SOC Home Lab environment.

The integration of Microsoft Sysmon and the Wazuh Agent established endpoint-level visibility and enabled the forwarding of security-related telemetry to the central Wazuh Manager.

This configuration provides the foundation for subsequent SOC activities, including:

- Security event monitoring and analysis.
- Endpoint threat detection.
- Log investigation and correlation.
- Attack simulation and detection testing.
- Incident investigation and response.

---

## 7. Screenshots and Evidence

The following screenshots provide evidence of the deployment and configuration process:

1. Windows 11 virtual machine system information.
2. Windows network adapter configuration and IP addresses.
3. Successful Sysmon installation.
4. Sysmon service running.
5. Sysmon Operational event logs in Event Viewer.
6. Wazuh Agent installation and configuration.
7. Wazuh Agent service status.
8. Windows endpoint successfully registered in the Wazuh Dashboard.
9. Sysmon events visible in the Wazuh Dashboard.
