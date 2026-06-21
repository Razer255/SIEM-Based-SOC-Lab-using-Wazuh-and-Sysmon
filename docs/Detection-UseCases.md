# Detection Use Cases

## Overview

This document outlines security detection use cases implemented and tested within the SIEM-Based SOC Lab using Wazuh, Sysmon, and Windows Event Logs.

The objective is to simulate common attacker behaviors and validate whether the SIEM can successfully detect and alert on suspicious activities.

---

# Use Case 1: Suspicious PowerShell Execution

## Description

Attackers frequently use PowerShell to execute malicious commands, download payloads, and establish persistence.

## Detection Source

* Sysmon Event ID 1
* Windows Event Logs
* Wazuh Rules

## Example Command

```powershell
powershell.exe -ExecutionPolicy Bypass
```

## Expected Alert

* Suspicious PowerShell activity detected
* MITRE ATT&CK mapping generated

## MITRE ATT&CK

| Technique | Name       |
| --------- | ---------- |
| T1059.001 | PowerShell |

---

# Use Case 2: Account Discovery

## Description

Threat actors often enumerate user accounts after gaining access.

## Detection Source

* Sysmon Process Creation Events
* Wazuh Rules

## Example Command

```cmd
net user
```

## Expected Alert

Detection of account enumeration activity.

## MITRE ATT&CK

| Technique | Name              |
| --------- | ----------------- |
| T1087     | Account Discovery |

---

# Use Case 3: Local Group Enumeration

## Description

Attackers identify privileged groups to target administrative accounts.

## Example Command

```cmd
net localgroup administrators
```

## Expected Alert

Privilege group enumeration detected.

## MITRE ATT&CK

| Technique | Name                   |
| --------- | ---------------------- |
| T1069.001 | Local Groups Discovery |

---

# Use Case 4: Network Configuration Discovery

## Description

Attackers gather network information to understand the environment.

## Example Command

```cmd
ipconfig /all
```

## Expected Alert

Network discovery activity identified.

## MITRE ATT&CK

| Technique | Name                                   |
| --------- | -------------------------------------- |
| T1016     | System Network Configuration Discovery |

---

# Use Case 5: DNS Discovery

## Description

Attackers use DNS tools to identify internal and external systems.

## Example Command

```cmd
nslookup google.com
```

## Expected Alert

DNS reconnaissance activity detected.

## MITRE ATT&CK

| Technique | Name                    |
| --------- | ----------------------- |
| T1018     | Remote System Discovery |

---

# Use Case 6: Data Obfuscation

## Description

Attackers attempt to conceal data within files to evade detection.

## Test Method

Atomic Red Team Test

### Technique

Steganographic Tarball Embedding

## Expected Alert

Suspicious file activity detected by Sysmon and Wazuh.

## MITRE ATT&CK

| Technique | Name             |
| --------- | ---------------- |
| T1001.002 | Data Obfuscation |

---

# Use Case 7: File Creation Monitoring

## Description

Monitor suspicious file creation within user directories.

## Detection Source

Sysmon Event ID 11

## Example

Creation of executables in:

```text
Downloads
Desktop
Temp
```

## Expected Alert

Unauthorized executable creation detected.

## MITRE ATT&CK

| Technique | Name           |
| --------- | -------------- |
| T1204     | User Execution |

---

# Use Case 8: Command Shell Execution

## Description

Attackers commonly launch command shells to perform actions on the system.

## Example Command

```cmd
cmd.exe
```

## Expected Alert

Command shell execution logged and monitored.

## MITRE ATT&CK

| Technique | Name                  |
| --------- | --------------------- |
| T1059.003 | Windows Command Shell |

---

# Use Case 9: Registry Modification

## Description

Registry modifications may indicate persistence mechanisms.

## Detection Source

Sysmon Event ID 13

## Expected Alert

Registry key modification detected.

## MITRE ATT&CK

| Technique | Name            |
| --------- | --------------- |
| T1112     | Modify Registry |

---

# Use Case 10: Persistence Through Startup Folder

## Description

Attackers place malicious files in startup locations to execute automatically after reboot.

## Example Location

```text
C:\Users\User\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
```

## Expected Alert

Persistence mechanism identified.

## MITRE ATT&CK

| Technique | Name                               |
| --------- | ---------------------------------- |
| T1547.001 | Registry Run Keys / Startup Folder |

---

# Alert Validation Process

For every use case:

1. Execute the test activity.
2. Generate endpoint logs.
3. Collect logs through Wazuh Agent.
4. Analyze logs using Wazuh Manager.
5. Trigger detection rule.
6. Verify alert in Wazuh Dashboard.
7. Document findings.

---

# Detection Summary

| Use Case                 | Technique ID |
| ------------------------ | ------------ |
| PowerShell Execution     | T1059.001    |
| Account Discovery        | T1087        |
| Group Enumeration        | T1069.001    |
| Network Discovery        | T1016        |
| DNS Discovery            | T1018        |
| Data Obfuscation         | T1001.002    |
| File Creation Monitoring | T1204        |
| Command Shell Execution  | T1059.003    |
| Registry Modification    | T1112        |
| Persistence              | T1547.001    |

---

# Conclusion

These detection use cases demonstrate the capability of the SIEM-Based SOC Lab to identify common adversary techniques mapped to the MITRE ATT&CK framework. The lab provides practical experience in security monitoring, threat hunting, alert validation, and incident investigation using Wazuh and Sysmon.
