# VM Domain Join Failure Due to SID Conflict

Diagnosed and fixed a critical Active Directory domain join issue by resolving a duplicate SID conflict in a cloned virtual machine.

## Problem
- A virtual machine could not join the domain because it had the same SID as another machine in the domain.

## Investigation
1. Attempted to join the VM to the domain and received an error.
2. Reviewed the error message indicating a duplicate SID conflict.
3. Confirmed the VM was cloned from a base image without generalization.
4. Verified DNS and domain connectivity using `ipconfig` and `nslookup`.

## Tools Used
- VMware Workstation  
- Windows Server System Properties  
- Domain Join Dialog  
- DNS Manager  
- Command Prompt  
- PowerShell  
- `ipconfig`  
- `nslookup`  

## Solution
1. Opened the Sysprep tool on the cloned VM.
2. Selected **OOBE (Out-of-Box Experience)**.
3. Checked the **Generalize** option.
4. Restarted the VM to generate a new SID.
5. Attempted the domain join again.
6. Successfully joined the VM to the domain.

## What I Learned
1. Cloning a Windows VM without generalizing it can cause SID conflicts.
2. Each machine in a domain must have a unique SID.
3. Running Sysprep with the Generalize option ensures the VM can safely join a domain.