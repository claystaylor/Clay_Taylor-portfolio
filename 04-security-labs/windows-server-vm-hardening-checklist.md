System Hardening Checklist for a Windows Server Virtual Machine

System Description
This checklist is for a Windows Server virtual machine used for class labs and basic server practice in VMware Workstation.

Checklist

Enable Windows updates.
Restrict remote access to only what is needed.
Use a strong administrator password.
Enable and verify Windows Firewall.
Disable or remove unused roles, features, and services.
Use only the network adapters the VM actually needs.
Configure backups or take clean VM snapshots before major changes.
Enable BitLocker or other encryption if the VM stores sensitive data.

What risks still remain?

User mistakes can still weaken security.
If the host computer is compromised, the VM can still be at risk.
Weak network design can still expose the VM.