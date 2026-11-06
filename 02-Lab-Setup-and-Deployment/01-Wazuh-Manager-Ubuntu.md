# 01 - Wazuh Manager (Ubuntu Server 22.04 LTS) Setup

## 1. Overview

This document outlines the installation, network configuration, and service verification of the Wazuh SIEM Manager within an isolated enterprise SOC home lab.

The Ubuntu Server 22.04 LTS virtual machine serves as the centralized security monitoring node, responsible for receiving endpoint telemetry, analyzing security events, managing connected agents, and providing security monitoring capabilities through the Wazuh Dashboard.

## 2. Infrastructure Specifications

| Component | Configuration |
|---|---|
| Operating System | Ubuntu Server 22.04 LTS |
| Role | Centralized SIEM Manager |
| SIEM Platform | Wazuh |
| Virtualization | Oracle VM VirtualBox |
| Network Mode | Host-Only Adapter |
| Host-Only IP | `192.168.56.102` |
| Subnet | `192.168.56.0/24` |

### Port Allocations

The following ports are used by the Wazuh environment:

| Port | Protocol | Purpose |
|---|---|---|
| 1514 | TCP | Wazuh Agent communication |
| 1515 | TCP | Wazuh Agent enrollment |
| 443 | TCP | Wazuh Dashboard access (HTTPS) |
| 55000 | TCP | Wazuh Server API |

Note: Port availability and configuration should be verified against the deployed Wazuh services and network configuration.

---

## 3. Installation and Deployment Steps

### Step 1: System Update and Prerequisites

The Ubuntu server was updated to ensure that the operating system packages were current before deploying the Wazuh platform.

The following commands were executed:

Update package repositories and upgrade installed packages:

```bash
sudo apt update && sudo apt upgrade -y
```

Install the required utilities:

```bash
sudo apt install -y curl apt-transport-https lsb-release gnupg
```

### Step 2: Wazuh Installation and Deployment

The Wazuh SIEM platform was deployed on the Ubuntu Server using the official Wazuh installation procedure.

The deployment establishes the following core components:

- **Wazuh Manager:** Receives and analyzes security events from connected endpoints.
- **Wazuh Indexer:** Stores and indexes security event data for searching and analysis.
- **Wazuh Dashboard:** Provides the web-based interface for security monitoring and investigation.

The official installation documentation was used as a reference:

https://documentation.wazuh.com/current/quickstart.html

### Step 3: Network Configuration

The Ubuntu virtual machine was configured to communicate with the other virtual machines through the VirtualBox Host-Only network.

The server's assigned IP address was checked using:

```bash
ip addr
```

The routing table was inspected using:

```bash
ip route
```

The expected server IP address was:

`192.168.56.102`

This address is used by the Windows endpoint when configuring its connection to the Wazuh Manager.

### Step 4: Wazuh Service Verification

After deployment, the status of the Wazuh services was checked to confirm that the components were running.

Check the Wazuh Manager:

```bash
sudo systemctl status wazuh-manager
```

Check the Wazuh Indexer:

```bash
sudo systemctl status wazuh-indexer
```

Check the Wazuh Dashboard:

```bash
sudo systemctl status wazuh-dashboard
```

### Step 5: Network Port Verification

The listening ports were inspected to verify that the required services were available.

```bash
sudo ss -tulpn
```

To filter for the main Wazuh ports:

```bash
sudo ss -tulpn | grep -E '1514|1515|443|55000'
```

The expected ports include:

- 1514/TCP – Agent communication.
- 1515/TCP – Agent enrollment.
- 443/TCP – Dashboard access.
- 55000/TCP – Wazuh API.

Actual listening ports depend on the services enabled and their configuration.

### Step 6: Wazuh Dashboard Access

The Wazuh Dashboard was accessed through a web browser using the server's IP address.

Dashboard URL:

`https://192.168.56.102`

The dashboard provides access to:

- Security event monitoring.
- Connected endpoint management.
- Security alerts and event analysis.
- Vulnerability detection.
- Agent status monitoring.
- Security investigation tools.

---

## 4. Endpoint Integration Preparation

The Ubuntu server was prepared to receive security telemetry from the Windows 11 endpoint.

The Wazuh Manager acts as the central collection and analysis point, while the Wazuh Agent installed on Windows 11 facilitates communication between the endpoint and the server.

The Windows endpoint is configured to communicate with the Wazuh Manager using:

`192.168.56.102`

Agent connectivity and registration are verified through the Wazuh Dashboard.

---

## 5. Screenshots and Technical Evidence

The following screenshots provide evidence of the deployment and configuration process:

1. Ubuntu Server 22.04 LTS running in VirtualBox.
2. Ubuntu IP address configuration showing `192.168.56.102`.
3. System package updates and prerequisite installation.
4. Wazuh installation process.
5. Wazuh Manager service status.
6. Wazuh Indexer and Dashboard service status.
7. Listening network ports.
8. Successful access to the Wazuh Dashboard.

Screenshots are stored in the repository's `assets/screenshots/` directory.

---

## 6. Challenges Encountered

Document any challenges experienced during deployment, including:

1. Package Installation Errors
Challenge: Encountered errors when installing required packages and dependencies on the Ubuntu Server.

Troubleshooting and Resolution:
- Updated the system package repositories using sudo apt update.
- Upgraded existing packages using sudo apt upgrade.
- Checked internet connectivity and repository availability.
- Re-attempted the installation after resolving package dependency issues.
2. Network Configuration Issues
Challenge: Experienced difficulties establishing communication between the Ubuntu Wazuh Manager and the Windows 11 endpoint.

Troubleshooting and Resolution:
- Inspected network interface configurations using ip addr.
- Verified IP addresses and network connectivity between virtual machines.
- Used ping to test communication between the endpoints.
- Ensured both virtual machines were configured on compatible VirtualBox network settings.
3. Wazuh Service Startup Failures
Challenge: Encountered difficulties starting or verifying Wazuh services after installation.

Troubleshooting and Resolution:
- Used systemctl status wazuh-manager to inspect the service status.
- Reviewed service logs to identify potential configuration or startup errors.
- Restarted the affected services after addressing identified issues.
- Verified successful startup using systemctl.
4. Wazuh Dashboard Connectivity Problems
Challenge: Experienced difficulties accessing the Wazuh Dashboard through the web browser.

Troubleshooting and Resolution:
- Verified that the Wazuh Dashboard service was running.
- Checked whether the dashboard was listening on its designated port (443).
- Verified the server IP address used to access the dashboard.
- Checked connectivity between the host machine and the Ubuntu Server.
5. Firewall and Port Accessibility
Challenge: Encountered potential connectivity restrictions when configuring communication between the Wazuh components.

Troubleshooting and Resolution:
- Inspected firewall configurations.
- Verified accessibility of the required Wazuh communication ports.
- Checked that the necessary services were listening on their designated ports.

---

## 7. Deployment Outcome

The Ubuntu Server was configured as the centralized Wazuh SIEM Manager within the SOC home lab.

The deployment establishes the foundation for centralized endpoint monitoring, security event collection, log analysis, and threat detection.

---

## 8. Lessons Learned

Through this deployment, I gained practical experience in:

- Ubuntu Server administration.
- SIEM installation and configuration.
- Linux service management using systemctl.
- Network configuration and port verification.
- Understanding Wazuh's core components.
- Establishing a centralized security monitoring environment.

