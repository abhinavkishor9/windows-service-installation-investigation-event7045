# Investigation Notes

## Objective

Investigate Windows Service Installation using Event ID 7045.

---

## Timeline

### Service Created

```
sc create SOCService
```

Service successfully installed.

---

### Service Verification

```
sc query SOCService
```

Confirmed service exists.

---

### Service Start

```
sc start SOCService
```

Returned:

```
Error 1053
```

Reason:

The configured binary (`cmd.exe /c exit`) terminates immediately and never reports a running service state.

---

### Event Generated

Event ID:

7045

Log:

System

Source:

Service Control Manager

---

## Event Analysis

Observed fields:

Service Name

```
SOCService
```

Service File

```
C:\Windows\System32\cmd.exe /c exit
```

Start Type

```
Demand Start
```

Service Account

```
LocalSystem
```

Service Type

```
User mode service
```

---

## PowerShell Investigation

Executed:

```powershell
Get-WinEvent -LogName System |
Where-Object {$_.Id -eq 7045}
```

Successfully retrieved Event ID 7045.

---

## Service Configuration

Executed:

```
sc qc SOCService
```

Observed:

- Binary Path
- Start Type
- LocalSystem Account
- Error Control

---

## Cleanup

Executed:

```
sc stop SOCService

sc delete SOCService
```

Verification:

```
sc query SOCService
```

Returned:

```
FAILED 1060
```

Service successfully removed.

---

## SOC Analysis

Although this service was benign, analysts should always investigate:

- Executable path
- Service account
- Parent installation process
- Creation time
- Digital signature
- File hash
- Persistence mechanism

These characteristics help distinguish legitimate software installations from attacker persistence.
