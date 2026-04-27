# Simulated Security Events

All simulations were performed within the isolated `siem-lab-net` virtual environment on either the Windows or Linux endpoint. The Wazuh dashboard was monitored throughout each simulation to confirm detection and alert generation. No external systems or unauthorized targets were involved.

---

## Simulation 1 — Brute Force Authentication Attack (Windows Endpoint)

**Target:** Windows endpoint — user account "Jane Doe"
**MITRE ATT&CK:** T1110.001 — Brute Force: Password Guessing

### Method

Repeated failed login attempts were manually entered against the Windows endpoint login screen using incorrect credentials. After multiple failures, a successful login was performed to simulate a successful brute-force compromise.

The Windows login screen displayed "Invalid credentials, delaying next attempt..." after each failed attempt, confirming the failed authentication events were being generated.

### Wazuh Detection

The SIEM generated multiple authentication-related alerts showing the pattern of repeated failures followed by a successful login. These events were visible in the Wazuh Dashboard and provided enough information to:

- Identify the affected endpoint
- Identify the targeted account
- Observe the rapid sequence of failures
- Confirm the eventual successful authentication

### Defensive Relevance

This simulation demonstrates why account lockout policies are a critical control. Without lockout enforcement, an attacker can attempt unlimited passwords against an account. The SIEM's ability to surface this pattern enables a SOC analyst to detect and respond before significant access is achieved.

---

## Simulation 2 — Firewall Disablement (Windows Endpoint)

**Target:** Windows endpoint
**MITRE ATT&CK:** T1562.004 — Impair Defenses: Disable or Modify System Firewall

### Method

Windows Firewall was disabled across all profiles using the following PowerShell command run from an Administrator PowerShell session:

```powershell
Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False
```

This command disables firewall protection across Domain, Public, and Private network profiles simultaneously — simulating how an attacker with administrative access might weaken host defenses to enable further activity.

### Wazuh Detection

Wazuh generated a security policy change alert corresponding to the firewall modification event. This alert would allow a SOC analyst to identify that a host's firewall was disabled and trigger an immediate investigation into whether the change was authorized.

### Defensive Relevance

Firewall disablement is a common attacker technique used to reduce detection visibility and allow outbound communication for C2 or data exfiltration. Detecting this event in near-real-time is critical for preventing follow-on attack stages.

---

## Simulation 3 — Privilege Escalation via Account Creation (Windows Endpoint)

**Target:** Windows endpoint
**MITRE ATT&CK:** T1136.001 — Create Account: Local Account / T1098 — Account Manipulation

### Method

A new user account was created on the Windows endpoint and then added to the local Administrators group, simulating an attacker establishing a persistent administrative foothold.

### Wazuh Detection

Wazuh generated a high-severity alert (Rule ID: 60154, Rule Level: 12) with the description "Administrators Group Changed." The alert was visible in the Wazuh Discover view with the following key fields:

| Field | Value |
|-------|-------|
| rule.id | 60154 |
| rule.level | 12 |
| rule.description | Administrators Group Changed |
| rule.groups | windows, windows_security, group_changed, win_group_changed |
| input.type | log |
| location | EventChannel |

Rule level 12 is classified as a high-severity event in Wazuh, meaning it would surface prominently in a SOC analyst's dashboard and warrant immediate investigation.

### Defensive Relevance

Unauthorized account creation combined with privilege escalation is a textbook persistence technique. The SIEM's ability to detect and alert on administrator group changes in near-real-time provides the SOC team with an early opportunity to identify and remediate unauthorized access before the attacker can leverage the new account.

---

## Simulation 4 — File Permission and System Manipulation (Linux Endpoint)

**Target:** Linux endpoint — user account "joe-doe"
**MITRE ATT&CK:** T1222.002 — File and Directory Permissions Modification: Linux and Mac

### Method

The following actions were performed on the Linux endpoint to simulate post-compromise system manipulation:

```bash
# Attempted privilege escalation via su
su root          # repeated multiple times — all attempts failed (Authentication failure)

# SSH service manipulation
sudo systemctl stop ssh
sudo systemctl start ssh

# File permission modification on sensitive file
chmod 777 ~/secret.txt    # world-readable/writable — overly permissive
chmod 600 ~/secret.txt    # restricted back — simulating attacker covering tracks
```

The `sudo` commands generated three incorrect password attempt lockout messages, and the `chmod` operations on `~/secret.txt` triggered Wazuh file integrity monitoring alerts.

### Wazuh Detection

Wazuh's File Integrity Monitoring (FIM) module detected the permission changes on the monitored file. The Wazuh dashboard simultaneously showed the alert firing alongside the terminal activity, visible in the event timeline.

Additional alerts were generated for:
- Repeated `su root` authentication failures
- SSH service stop/start events via systemctl

### Defensive Relevance

File permission changes on sensitive files, combined with repeated privilege escalation attempts, are strong indicators of post-compromise activity. Wazuh's FIM module provides continuous monitoring of file attributes, enabling detection of both permission changes and content modifications without requiring direct endpoint access.

---

## Summary of Detection Results

| Simulation | MITRE Technique | Wazuh Detected | Alert Level |
|------------|----------------|----------------|-------------|
| Brute Force Login | T1110.001 | ✅ Yes | High |
| Firewall Disablement | T1562.004 | ✅ Yes | High |
| Account Creation + Privilege Escalation | T1136.001 / T1098 | ✅ Yes | High (Level 12) |
| File Permission Modification | T1222.002 | ✅ Yes | Medium–High |

All four simulation categories were successfully detected by the Wazuh SIEM, demonstrating meaningful security visibility across authentication, defense evasion, persistence, and file integrity attack categories.

---

*All simulations were performed in a controlled, isolated virtual environment for educational purposes. No production systems or unauthorized targets were involved.*
