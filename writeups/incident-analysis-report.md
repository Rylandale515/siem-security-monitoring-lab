# Incident Analysis Report
**SIEM Monitoring Lab – Capstone Project**

*Rylan Stalnaker | Western Governors University*

---

# 1. Incident Details

**Date/Time:** February 22, 2026 – 19:30 UTC
**Location:** Simulated organizational network – Tahlequah, Oklahoma (lab environment)
**Severity:** High
**Status:** Contained and Remediated

Two endpoint systems within the lab environment were compromised via a brute-force password attack targeting a Windows workstation. Following initial access, the attacker performed privilege escalation, established persistence, and modified system defenses.

The Wazuh SIEM platform successfully detected and logged indicators of compromise throughout the attack lifecycle.

---

# 2. Incident Classification

**Severity:** High
**Estimated CVSS Score:** 8.5

| Impact Category | Rating |
|-----------------|--------|
| Confidentiality | High |
| Integrity | High |
| Availability | Low |

The compromise was contained to two endpoint systems and did not affect the Wazuh SIEM server or critical infrastructure.

---

# 3. Incident Response Team

**Primary Investigator:** Rylan Stalnaker
**Role:** Lead Security Analyst – Capstone Investigation

*Note: Contact details are fictional and included for simulation purposes.*

---

# NIST SP 800-61 Phase 1 – Preparation

The lab environment was configured in advance with the following controls and monitoring capabilities:

- Wazuh SIEM deployed on Ubuntu Server with agents on Windows and Linux endpoints
- Centralized log collection across both endpoints
- File integrity monitoring (FIM) enabled on sensitive directories
- Authentication logging enabled for Windows Security Event Log
- Baseline activity documented prior to attack simulation

---

# NIST SP 800-61 Phase 2 – Detection and Analysis

## Initial Detection

The incident was detected through automated alerting in the Wazuh SIEM dashboard at **19:46 UTC**, approximately 16 minutes after the attack began.

### Alerts Triggered

| Alert Type | Source | Wazuh Rule Category |
|------------|--------|---------------------|
| Multiple failed authentication attempts | Windows Security Log | Authentication |
| Successful login following repeated failures | Windows Security Log | Authentication |
| New administrative account creation | Windows Event Log | Account Management |
| Firewall policy modification | Windows Event Log | Policy Change |
| Scheduled task creation | Windows Event Log | Persistence |
| File permission change on sensitive path | Linux FIM | File Integrity |

### Evidence Collected

- Wazuh SIEM alert logs and dashboard captures
- Windows Security Event Log
- Linux file integrity monitoring alerts
- Administrative account creation events
- Firewall and scheduled task configuration logs

### Timeline of Observed Events

| Time (UTC) | Event |
|------------|-------|
| 19:30 | Brute-force authentication attack begins on Windows endpoint |
| 19:34 | Successful authentication achieved after repeated failures |
| 19:36 | New Active Directory user account created |
| 19:37 | Account added to local Administrators group |
| 19:38 | Windows Firewall rules modified via PowerShell |
| 19:39 | Scheduled task created for persistence |
| 19:40 | File permissions modified on sensitive files (Linux endpoint) |
| 19:46 | SIEM alert reviewed and escalated by SOC analyst |
| 19:50 | Incident formally declared and response initiated |

---

# NIST SP 800-61 Phase 2 – MITRE ATT&CK Mapping

Selected observed behaviors were mapped to the MITRE ATT&CK framework to support structured analysis and connect lab activity to real-world adversary techniques.

| Technique ID | Sub-Technique | Technique Name | Observed Behavior | Log Evidence |
|--------------|---------------|----------------|-------------------|--------------|
| T1110.001 | Password Guessing | Brute Force | Repeated failed login attempts against a single Windows account in a short timeframe, followed by a successful authentication | Windows Security Event (failed logon) repeated in sequence; followed by Event (successful logon) — surfaced as Wazuh authentication anomaly alert |
| T1136.001 | Local Account | Create Account | A new user account was created on the Windows endpoint immediately following initial access | Windows Security Event (user account created) — flagged by Wazuh account management rule |
| T1098 | — | Account Manipulation | Newly created account was added to the local Administrators group, granting elevated privileges | Windows Security Event (member added to security-enabled local group) — Wazuh privilege escalation alert |
| T1562.004 | Disable or Modify System Firewall | Impair Defenses | Windows Firewall was disabled via PowerShell to reduce detection and allow broader network access | Windows Security Event (security policy change) — Wazuh system configuration alert |
| T1053.005 | Scheduled Task | Scheduled Task/Job | A scheduled task was created to maintain persistent access to the compromised endpoint | Windows Security Event (scheduled task created) — Wazuh persistence detection alert |
| T1222.002 | Linux and Mac File and Directory Permissions Modification | File and Directory Permissions Modification | Permissions on sensitive files were modified on the Linux endpoint to allow broader access using `chmod 777 ~/secret.txt` & `chmod 600 ~/secret.txt` | Wazuh File Integrity Monitoring (FIM) alert — detected attribute change on monitored file path |

---

# NIST SP 800-61 Phase 3 – Containment, Eradication, and Recovery

## Containment

Actions taken immediately upon incident confirmation:

1. Both compromised endpoints were isolated from the virtual network
2. Network interfaces were administratively disabled to prevent lateral movement
3. Compromised administrative credentials were disabled
4. SIEM monitoring frequency was increased across remaining systems

## Eradication

Following containment, the following eradication steps were performed:

- Unauthorized administrative accounts were removed
- Malicious scheduled tasks were deleted
- Firewall rules were restored to pre-incident configuration
- Account lockout policies were applied to prevent future brute-force success
- Least-privilege access controls were enforced across both endpoints
- Systems were restored from verified clean snapshots

## Recovery

- Both endpoints were restored from pre-compromise backups
- System integrity was verified post-restoration
- Operational functionality was confirmed before returning endpoints to the network
- Hardened security configurations were applied to both endpoints

---

# NIST SP 800-61 Phase 4 – Post-Incident Activity

## Lessons Learned

| Finding | Impact | Recommendation |
|---------|--------|----------------|
| No account lockout policy enforced | Allowed unlimited brute-force attempts | Implement account lockout after 5 failed attempts |
| Username visible in login screen | Reduced brute-force effort required | Configure login screen to require manual username entry |
| No MFA on Windows endpoint | Single-factor compromise was sufficient | Deploy MFA for all administrative and remote access |
| Firewall modification was not alerted in real time | Delayed detection of defense evasion | Tune Wazuh rule for immediate alerting on firewall policy changes |
| Scheduled task creation not immediately correlated | Persistence went undetected until manual review | Create correlation rule linking account creation + task creation within short window |

## Recommendations

1. Enforce account lockout policies across all endpoints
2. Implement multi-factor authentication for all privileged accounts
3. Configure login screens to prevent username enumeration
4. Develop Wazuh correlation rules for chained attack behaviors (e.g., brute force → new account → scheduled task)
5. Conduct regular incident response simulations to validate detection coverage
6. Perform periodic reviews of scheduled tasks, local administrator group membership, and firewall rules

---

# 11. Legal and Regulatory Notes

This incident occurred entirely within a controlled virtual lab environment for educational purposes. No production systems, real user data, or unauthorized targets were involved. In a production environment, incidents of this nature may trigger regulatory notification requirements depending on applicable compliance frameworks (e.g., HIPAA, PCI-DSS, NIST CSF).

---

# 12. Estimated Response Costs (Simulated)

| Category | Estimated Cost |
|----------|----------------|
| Incident Investigation | $2,000 |
| System Recovery | $1,500 |
| Security Control Improvements | $1,000 |
| **Total** | **$4,500** |

---

*This report was produced as part of a capstone SIEM lab project. All scenarios were simulated in an isolated virtual environment. Documentation methodology is aligned with NIST SP 800-61 Rev. 2 and MITRE ATT&CK v14.*
