# SIEM-Based SOC Lab Architecture

## Overview

This project demonstrates the implementation of a Security Information and Event Management (SIEM) lab using Wazuh, Sysmon, Windows, and Ubuntu. The objective is to simulate a real-world Security Operations Center (SOC) environment for log collection, monitoring, threat detection, incident analysis, and alert generation.

---

## Architecture Diagram

```text
+---------------------+
|  Windows 10 VM      |
|---------------------|
| Sysmon              |
| Wazuh Agent         |
+----------+----------+
           |
           | Event Logs
           v
+---------------------+
| Ubuntu Server VM    |
|---------------------|
| Wazuh Manager       |
| Filebeat            |
| Elasticsearch       |
| Kibana Dashboard    |
+----------+----------+
           |
           | Alerts & Analytics
           v
+---------------------+
| SOC Analyst         |
|---------------------|
| Monitoring          |
| Threat Hunting      |
| Incident Response   |
+---------------------+
```

---

## Components

### 1. Windows 10 Endpoint

The Windows 10 virtual machine acts as the monitored endpoint.

#### Installed Components

* Sysmon
* Wazuh Agent
* Windows Event Logging

#### Responsibilities

* Generate security logs
* Monitor process creation
* Track network connections
* Detect file modifications
* Forward logs to Wazuh Manager

---

### 2. Sysmon

System Monitor (Sysmon) extends Windows logging capabilities by recording detailed endpoint activities.

#### Monitored Events

| Event ID | Description           |
| -------- | --------------------- |
| 1        | Process Creation      |
| 3        | Network Connection    |
| 7        | Image Loaded          |
| 11       | File Created          |
| 13       | Registry Modification |
| 22       | DNS Queries           |

#### Benefits

* Enhanced visibility
* Threat hunting support
* Malware detection
* Forensic investigations

---

### 3. Wazuh Agent

The Wazuh Agent collects security events from the Windows endpoint and securely forwards them to the Wazuh Manager.

#### Functions

* Log collection
* Integrity monitoring
* Vulnerability detection
* Security event forwarding

---

### 4. Ubuntu Server

The Ubuntu virtual machine hosts the SIEM infrastructure.

#### Installed Services

* Wazuh Manager
* Elasticsearch
* Filebeat
* Kibana
* Wazuh Dashboard

#### Responsibilities

* Receive endpoint logs
* Correlate security events
* Generate alerts
* Store security data
* Visualize findings

---

### 5. Wazuh Manager

The Wazuh Manager serves as the central analysis engine.

#### Core Functions

* Log analysis
* Rule matching
* Event correlation
* Alert generation
* Agent management

#### Detection Examples

* PowerShell abuse
* Privilege escalation attempts
* Suspicious process execution
* Persistence techniques
* MITRE ATT&CK mapping

---

### 6. Wazuh Dashboard

The dashboard provides a centralized interface for security monitoring.

#### Features

* Alert monitoring
* Agent status tracking
* Threat intelligence visibility
* Security analytics
* MITRE ATT&CK visualization

---

## Data Flow

### Step 1

User activity occurs on the Windows endpoint.

### Step 2

Sysmon records detailed system events.

### Step 3

Windows Event Logs are collected by the Wazuh Agent.

### Step 4

The Wazuh Agent securely forwards logs to the Wazuh Manager.

### Step 5

The Wazuh Manager processes logs against detection rules.

### Step 6

Alerts are generated when suspicious behavior is detected.

### Step 7

Alerts are stored in Elasticsearch.

### Step 8

The Wazuh Dashboard visualizes security findings for analysis.

---

## Attack Simulation

To validate detections, Atomic Red Team tests are executed on the Windows endpoint.

### Examples

* T1059 – Command and Scripting Interpreter
* T1087 – Account Discovery
* T1001.002 – Data Obfuscation
* T1105 – Ingress Tool Transfer
* T1547 – Boot or Logon Autostart Execution

Generated events are monitored through Wazuh to verify detection capabilities.

---

## Security Monitoring Workflow

1. Event Generated
2. Event Collected
3. Event Analyzed
4. Rule Triggered
5. Alert Created
6. Investigation Initiated
7. Incident Documented

---

## Future Improvements

* Integrate Suricata IDS
* Add Threat Intelligence Feeds
* Configure Active Response
* Implement Sigma Rules
* Add Multiple Endpoints
* Deploy Dockerized Components
* Create Automated Incident Reports

---

## Conclusion

This SIEM-Based SOC Lab provides hands-on experience with endpoint monitoring, threat detection, security event analysis, and incident response using Wazuh and Sysmon. The lab simulates a practical SOC environment and serves as a foundation for cybersecurity learning, threat hunting, and blue-team operations.
