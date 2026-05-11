# Event Log Analysis

# Log Source
- Windows Event Viewer
- TPM-WMI

# What happened?
- Error: The Secure Boot update failed to update SBAT with error Unknown HResult Error code: 0x800700c1
- Event ID: 1796
- Source: TPM-WMI
- Date and Time: 5/10/2026 10:14:27 PM

Note: A second TPM-WMI Event ID 1796 error appeared earlier the same day at 05/10/2026 12:17:56 PM. If the error continues after restart, I would investigate further.

# Is it Normal or Suspicious?
- Likely normal. This sometimes happens when a Secure Boot update fails to apply. Windows may try to apply the update at the next system restart.
- System-level process, not something triggered by a user.
- I didn't find any Windows Update errors, so there's no clear sign this was suspicious.

# What did I learn?
- Sometimes a system update fails but Windows may retry the update at the next system reboot. This shows the importance of restarting my computer regularly.

- SBAT (Secure Boot Advanced Targeting) is a blocklist for your computer's startup process. It tells your PC which old, vulnerable boot software to reject, so they can't run during startup even if they look legitimate. Keeping the blocklist updated is important because it prevents known vulnerable bootloaders from running.

- Event Viewer, when used properly, is very useful for discovering potential risks and reducing guesswork.

# Tools to investigate further, additional logs, and false positive causes

1. Check Windows Logs > System
    - Main location of the event/error.

2. Kernel-Boot > Operational
    - Check because the error involves Secure Boot, SBAT, and the boot process.

3. WindowsUpdateClient > Operational
    - See if Windows Update triggered it.

4. False positive causes
    - Pending restart
    - Windows trying to update again later
    - Outdated BIOS/UEFI firmware
    - Temporary Secure Boot or TPM issue