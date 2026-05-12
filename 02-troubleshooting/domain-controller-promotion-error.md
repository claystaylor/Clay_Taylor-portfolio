```md
# DC21 Domain Controller Promotion

## Overview
This issue was encountered during a VMware virtual server lab. DC21 is a cloned virtual machine
that was sysprepped to generate a unique SID, then configured as a member server before being
promoted to a secondary domain controller for the existing domain `TestDomain.local`.

Prior to promotion, the following was configured on DC21:
- C: drive was mirrored for redundancy
- A second network adapter was added and a NIC team was created
- Sysprep was run to assign a unique SID (as DC21 was cloned from an existing VM)

The AD DS Configuration Wizard failed with a message saying it could not contact or verify
the existing domain controller/domain.

> **Note:** Running Sysprep on a cloned machine before domain joining can sometimes cause
> domain membership issues. However, this was investigated and ruled out as the cause of
> the promotion failure.

---

## What I Checked

### 1. Primary Domain Controller (DC20)
DC20 showed:

| Setting    | Value            |
|------------|------------------|
| Hostname   | DC20             |
| Domain     | TestDomain.local |
| IP Address | 192.168.42.20    |
| DNS Server | 192.168.42.20    |

That looked correct for a primary DC/DNS server.

---

### 2. DC21 Network Settings
DC21 showed:

| Setting         | Value            |
|-----------------|------------------|
| Hostname        | DC21             |
| Domain Suffix   | TestDomain.local |
| IP Address      | 192.168.42.21    |
| Default Gateway | 192.168.42.2     |
| DNS Server      | 192.168.42.20    |

DC21 was pointing to DC20 for DNS, which is correct before promotion.

---

### 3. Basic Connectivity Test (DC21 to DC20)
DC21 was able to reach:
- `dc20`
- `192.168.42.20`

Basic network communication was working.

---

### 4. DNS Resolution Test
```cmd
nslookup testdomain.local
```
Resolved to `192.168.42.20` — basic DNS lookup was working.

---

### 5. Domain Controller Discovery Test
```cmd
nltest /dsgetdc:testdomain.local
```
Successfully found:
- `DC20.TestDomain.local`
- `192.168.42.20`

DC21 could locate the existing domain controller.

---

## What I Found
- Network was working
- DNS was mostly working
- DC21 could find DC20
- The AD DS GUI wizard still failed with a **"Bad DNS packet"** error

The issue was not a simple IP address, gateway, or DNS server setting problem.

---

## What Fixed It
Instead of using the GUI wizard, the PowerShell promotion command was used to promote DC21
as an additional domain controller. That worked.

---

## Final Resolution
After confirming that DC21 could:
- Reach DC20
- Resolve the domain through DNS
- Locate the existing domain controller with `nltest`

PowerShell was used to successfully promote DC21 to a secondary domain controller.
```