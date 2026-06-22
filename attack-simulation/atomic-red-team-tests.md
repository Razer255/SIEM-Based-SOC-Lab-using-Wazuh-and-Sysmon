# Atomic Red Team Test Execution Report

## Overview

This document records Atomic Red Team simulations executed against the Windows endpoint in the SIEM-Based SOC Lab.

The purpose of these tests is to validate the ability of Wazuh and Sysmon to detect adversary behaviors mapped to the MITRE ATT&CK framework.

---

# Lab Environment

| Component           | Technology      |
| ------------------- | --------------- |
| SIEM                | Wazuh           |
| Endpoint Monitoring | Sysmon          |
| Endpoint            | Windows 10      |
| SIEM Server         | Ubuntu Server   |
| Attack Simulation   | Atomic Red Team |

---

# Testing Methodology

For each Atomic Test:

1. Execute the Atomic Red Team test.
2. Generate endpoint telemetry.
3. Collect logs through Wazuh Agent.
4. Analyze logs using Wazuh Manager.
5. Verify alert generation.
6. Map activity to MITRE ATT&CK.
7. Document findings.

---

# Test 1 – Account Discovery

## Technique

| ID    | Name              |
| ----- | ----------------- |
| T1087 | Account Discovery |

## Objective

Simulate an attacker enumerating local user accounts.

## Command

```powershell id="0r1n2s"
Invoke-AtomicTest T1087
```

## Expected Behavior

* User enumeration commands executed.
* Sysmon process creation events generated.
* Wazuh alerts triggered.

## Detection Result

| Control         | Result  |
| --------------- | ------- |
| Sysmon Logging  | Success |
| Wazuh Detection | Success |
| Alert Generated | Yes     |

## Status

✅ Passed

---

# Test 2 – PowerShell Execution

## Technique

| ID        | Name       |
| --------- | ---------- |
| T1059.001 | PowerShell |

## Objective

Simulate suspicious PowerShell activity.

## Command

```powershell id="h9n3xp"
Invoke-AtomicTest T1059.001
```

## Expected Detection

* PowerShell process creation
* Suspicious execution monitoring
* Wazuh alert generation

## Status

✅ Passed

---

# Test 3 – Network Discovery

## Technique

| ID    | Name                                   |
| ----- | -------------------------------------- |
| T1016 | System Network Configuration Discovery |

## Objective

Simulate collection of network information.

## Command

```powershell id="v4g2de"
Invoke-AtomicTest T1016
```

## Expected Detection

* Execution of networking commands
* Process creation logs
* Discovery alerts

## Status

✅ Passed

---

# Test 4 – Data Obfuscation

## Technique

| ID        | Name             |
| --------- | ---------------- |
| T1001.002 | Data Obfuscation |

## Objective

Validate detection of steganographic embedding techniques.

## Command

```powershell id="q7f1jc"
Invoke-AtomicTest T1001.002
```

## Expected Detection

* Suspicious file operations
* Process activity
* Wazuh alerts

## Status

✅ Passed

---

# Test 5 – Registry Modification

## Technique

| ID    | Name            |
| ----- | --------------- |
| T1112 | Modify Registry |

## Objective

Simulate registry modifications commonly used for persistence.

## Command

```powershell id="e5y8kt"
Invoke-AtomicTest T1112
```

## Expected Detection

* Registry change monitoring
* Sysmon Event ID 13 generation
* Wazuh alert creation

## Status

✅ Passed

---

# Test 6 – Persistence via Startup Folder

## Technique

| ID        | Name                               |
| --------- | ---------------------------------- |
| T1547.001 | Registry Run Keys / Startup Folder |

## Objective

Simulate persistence by adding files to startup locations.

## Command

```powershell id="m3k7pa"
Invoke-AtomicTest T1547.001
```

## Expected Detection

* File creation monitoring
* Persistence alert generation

## Status

✅ Passed

---

# Test Results Summary

| Technique ID | Technique Name    | Detection Status |
| ------------ | ----------------- | ---------------- |
| T1087        | Account Discovery | ✅ Detected       |
| T1059.001    | PowerShell        | ✅ Detected       |
| T1016        | Network Discovery | ✅ Detected       |
| T1001.002    | Data Obfuscation  | ✅ Detected       |
| T1112        | Modify Registry   | ✅ Detected       |
| T1547.001    | Persistence       | ✅ Detected       |

---

# Lessons Learned

* Sysmon provides detailed endpoint telemetry.
* Wazuh successfully ingests and correlates endpoint logs.
* MITRE ATT&CK mapping improves threat visibility.
* Atomic Red Team is effective for validating SIEM detections.
* Detection engineering can be tested safely in a lab environment.

---

# Future Testing

Planned Atomic Red Team simulations:

| Technique ID | Technique                       |
| ------------ | ------------------------------- |
| T1105        | Ingress Tool Transfer           |
| T1036        | Masquerading                    |
| T1027        | Obfuscated Files or Information |
| T1562        | Impair Defenses                 |
| T1070        | Indicator Removal on Host       |

---

# Conclusion

The Atomic Red Team simulations successfully validated the SIEM-Based SOC Lab's ability to detect and analyze common adversary techniques. Wazuh and Sysmon provided effective visibility into endpoint activity, while MITRE ATT&CK mapping enabled structured threat analysis. The results demonstrate practical SOC monitoring, detection validation, and incident investigation capabilities.
