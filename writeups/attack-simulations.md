# Simulated Security Events

To evaluate the effectiveness of the SIEM, several suspicious and malicious activities were simulated within the lab environment.

## Brute Force Login Simulation

Multiple failed login attempts were generated on the Windows endpoint to simulate a brute force authentication attack.

After repeated failures, a successful login was executed to simulate a compromised account.

## Firewall Configuration Change

The Windows firewall was temporarily disabled and re-enabled using PowerShell to simulate an attacker attempting to weaken security controls.

## Privilege Escalation

A new user account was created and added to the administrator group.

This simulated an attacker gaining persistence and elevated privileges on the system.

## File Integrity Modification

Sensitive files on the Linux endpoint were modified to trigger file integrity monitoring alerts.
