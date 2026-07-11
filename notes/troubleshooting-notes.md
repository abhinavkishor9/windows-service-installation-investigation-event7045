# Troubleshooting Notes

## Issue

```
Error 1053

The service did not respond to the start or control request in a timely fashion.
```

### Cause

The configured executable

```
cmd.exe /c exit
```

terminates immediately.

It is not a real Windows Service binary.

Expected behavior for this lab.

---

## Issue

```
FAILED 1060

The specified service does not exist.
```

### Cause

The service has already been removed.

Resolution:

Recreate the service.

```
sc create SOCService ...
```

---

## Issue

PowerShell

```
Get-Service SOCService
```

returns

```
Cannot find service
```

### Cause

Service deleted successfully.

---

## Issue

No Event ID 7045 found.

### Possible Causes

- Wrong Event Log
- Event filter incorrect
- Service creation failed

Verify using:

```
sc query SOCService
```

---

## Issue

```
Access Denied
```

while creating service.

### Resolution

Run Command Prompt or PowerShell as Administrator.

Verify:

```
whoami
```

or check Administrator privileges.

---

## Issue

```
sc stop SOCService
```

returns

```
FAILED 1062

Service has not been started.
```

### Cause

Expected.

The test service never successfully starts because it exits immediately.

---

## Issue

Event Viewer not showing latest events.

### Resolution

Click:

```
Refresh
```

or press

```
F5
```

---

## Key Learning

Windows records service installation regardless of whether the service successfully starts.

This makes Event ID 7045 an excellent detection source for persistence investigations.
