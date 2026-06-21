# SIEM-Based SOC Lab Setup Guide

## Overview

This guide explains how to build a Security Information and Event Management (SIEM) lab using Wazuh, Sysmon, Ubuntu Server, and Windows 10.

The objective is to create a practical SOC environment for monitoring, threat detection, incident analysis, and security investigations.

---

# Lab Architecture

```text
Windows 10 VM
│
├── Sysmon
├── Wazuh Agent
│
▼
Ubuntu Server VM
│
├── Wazuh Manager
├── Filebeat
├── Elasticsearch
├── Kibana
└── Wazuh Dashboard
```

---

# Prerequisites

## Hardware Requirements

| Component | Recommended      |
| --------- | ---------------- |
| CPU       | 4+ Cores         |
| RAM       | 8 GB Minimum     |
| Storage   | 40 GB Free Space |
| Internet  | Required         |

---

## Software Requirements

### Host Machine

* Windows 10 / Windows 11
* VMware Workstation or VirtualBox

### Virtual Machines

#### Ubuntu Server

* Ubuntu Server 22.04 LTS

#### Windows Endpoint

* Windows 10
* Sysmon
* Wazuh Agent

---

# Step 1: Create Virtual Machines

## Ubuntu Server VM

Recommended Configuration:

| Setting | Value          |
| ------- | -------------- |
| RAM     | 4 GB           |
| CPU     | 2 Cores        |
| Disk    | 40 GB          |
| Network | NAT or Bridged |

---

## Windows 10 VM

Recommended Configuration:

| Setting | Value                  |
| ------- | ---------------------- |
| RAM     | 4 GB                   |
| CPU     | 2 Cores                |
| Disk    | 40 GB                  |
| Network | Same Network as Ubuntu |

---

# Step 2: Install Ubuntu Server

Install Ubuntu Server 22.04 LTS using default settings.

Verify installation:

```bash
sudo apt update
sudo apt upgrade -y
```

Check IP Address:

```bash
ip a
```

Record the Ubuntu IP address.

Example:

```text
192.168.1.100
```

---

# Step 3: Install Wazuh Server

Update packages:

```bash
sudo apt update
sudo apt upgrade -y
```

Install Wazuh using the official installation assistant:

```bash
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
sudo bash wazuh-install.sh -a
```

The installation process deploys:

* Wazuh Manager
* Elasticsearch
* Filebeat
* Wazuh Dashboard

---

# Step 4: Verify Wazuh Installation

Check service status:

```bash
sudo systemctl status wazuh-manager
```

Verify dashboard access:

```text
https://<Ubuntu-IP>
```

Example:

```text
https://192.168.1.100
```

---

# Step 5: Install Sysmon on Windows

Download Sysmon from Microsoft Sysinternals.

Open Command Prompt as Administrator.

Install Sysmon:

```cmd
sysmon64.exe -accepteula -i
```

Verify installation:

```cmd
sysmon64.exe -s
```

---

# Step 6: Verify Sysmon Logs

Open:

```text
Event Viewer
```

Navigate to:

```text
Applications and Services Logs
└── Microsoft
    └── Windows
        └── Sysmon
            └── Operational
```

Confirm Sysmon events are being generated.

---

# Step 7: Install Wazuh Agent on Windows

Download Wazuh Agent.

Run installer.

During installation enter:

```text
Manager Address:
<Ubuntu-IP>
```

Example:

```text
192.168.1.100
```

---

# Step 8: Start Wazuh Agent

Open PowerShell as Administrator:

```powershell
Start-Service WazuhSvc
```

Verify:

```powershell
Get-Service WazuhSvc
```

Expected State:

```text
Running
```

---

# Step 9: Register Agent

On Ubuntu:

```bash
sudo /var/ossec/bin/agent_control -l
```

Expected Output:

```text
ID: 001
Name: Windows10
Status: Active
```

---

# Step 10: Verify Agent Connectivity

Check agent logs:

```bash
sudo tail -f /var/ossec/logs/ossec.log
```

Expected Output:

```text
Agent connected
```

---

# Step 11: Configure Sysmon Log Collection

Edit:

```text
C:\Program Files (x86)\ossec-agent\ossec.conf
```

Add:

```xml
<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>
```

Restart Agent:

```powershell
Restart-Service WazuhSvc
```

---

# Step 12: Generate Test Events

Open Command Prompt:

```cmd
net user
```

```cmd
ipconfig /all
```

```cmd
whoami
```

```cmd
nslookup google.com
```

These commands generate events visible in Wazuh.

---

# Step 13: Validate Alerts

Login to Wazuh Dashboard.

Navigate to:

```text
Security Events
```

Verify:

* Sysmon Events
* Windows Logs
* Agent Activity
* Security Alerts

---

# Step 14: Simulate Attacks

Install Atomic Red Team.

Execute tests such as:

```powershell
Invoke-AtomicTest T1087
```

```powershell
Invoke-AtomicTest T1001.002
```

```powershell
Invoke-AtomicTest T1059.001
```

Observe generated alerts inside Wazuh Dashboard.

---

# Troubleshooting

## Agent Not Connected

Restart agent:

```powershell
Restart-Service WazuhSvc
```

---

## Manager Not Running

```bash
sudo systemctl restart wazuh-manager
```

---

## Dashboard Not Accessible

Check:

```bash
sudo systemctl status wazuh-dashboard
```

---

## No Sysmon Events

Verify Sysmon:

```cmd
sysmon64.exe -s
```

Check Event Viewer logs.

---

# Expected Outcomes

After completing the setup:

* Wazuh Manager operational
* Windows Agent connected
* Sysmon collecting endpoint telemetry
* Alerts visible in Dashboard
* MITRE ATT&CK mappings available
* Attack simulations successfully detected

---

# Conclusion

This SIEM-Based SOC Lab provides hands-on experience with endpoint monitoring, log analysis, threat detection, attack simulation, and incident response using Wazuh and Sysmon. The environment replicates core SOC workflows and serves as a practical cybersecurity learning platform.
