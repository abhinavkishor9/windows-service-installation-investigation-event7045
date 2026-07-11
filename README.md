# windows-service-installation-investigation-event7045
## Overview

This SOC investigation demonstrates how Windows records the installation of new services using **Event ID 7045** from the **System** log.

Windows services are frequently abused by attackers to establish persistence or execute malicious payloads with elevated privileges. Security analysts routinely investigate newly installed services to determine whether they are legitimate or malicious.

This lab demonstrates the complete investigation workflow.

---

## Objective

- Create a custom Windows service
- Generate Event ID 7045
- Investigate the event using Event Viewer
- Investigate using PowerShell
- Inspect service configuration
- Understand important forensic artifacts
- Remove the service safely

---

## Environment

- Windows 10
- VMware Workstation
- PowerShell
- Event Viewer
- Service Control Manager

---

## MITRE ATT&CK Mapping

| Technique | ID |
|------------|----|
| Create or Modify System Process: Windows Service | T1543.003 |
| Boot or Logon Autostart Execution | T1547 |
| Persistence | TA0003 |
| Privilege Escalation | TA0004 |

---

## Lab Steps

### Step 1

Verify Administrator access.

### Step 2

Create a test Windows service.

```cmd
sc create SOCService binPath= "C:\Windows\System32\cmd.exe /c exit"
```

---

### Step 3

Verify service creation.

```cmd
sc query SOCService
```

---

### Step 4

Attempt to start the service.

```cmd
sc start SOCService
```

Expected:

Service fails with Error 1053 because cmd.exe immediately exits.

---

### Step 5

Open Event Viewer.

Navigate to

Event Viewer

→ Windows Logs

→ System

Filter:

Event ID

7045

---

### Step 6

Inspect Event ID 7045.

Important fields:

- Service Name
- Service File Name
- Service Type
- Service Start Type
- Service Account

---

### Step 7

Investigate using PowerShell.

```powershell
Get-WinEvent -LogName System |
Where-Object {$_.Id -eq 7045} |
Select-Object -First 1 |
Format-List
```

---

### Step 8

Inspect service configuration.

```cmd
sc qc SOCService
```

---

### Step 9

Cleanup.

```cmd
sc stop SOCService

sc delete SOCService
```

---

### Step 10

Verify removal.

```cmd
sc query SOCService
```

Expected:

```
FAILED 1060

The specified service does not exist.
```

---

## Detection Opportunities

Monitor for:

- Event ID 7045
- Unknown service names
- Suspicious service binaries
- Services launched from Temp folders
- PowerShell services
- cmd.exe services
- LOLBins registered as services

---

## Skills Learned

- Windows Service investigation
- Service Control Manager
- Event ID 7045 analysis
- PowerShell log investigation
- Windows persistence techniques
- SOC event triage
