# SIEM Security Monitoring Lab  
### WGU Capstone Project – Rylan Stalnaker

## Overview

This project focused on the design, implementation, and evaluation of a centralized Security Information and Event Management (SIEM) solution using **Wazuh** in a controlled virtual lab environment.

The purpose of the project was to demonstrate how centralized log monitoring can improve security visibility, detect suspicious behavior, and support faster incident response in a small organizational setting.

The environment included:

- **Ubuntu Server** running the Wazuh SIEM platform
- **Windows endpoint** with Wazuh agent installed
- **Linux endpoint** with Wazuh agent installed
- A **private NAT virtual network** to isolate testing from external systems

After deployment, baseline activity was generated and documented. Controlled suspicious and malicious behaviors were then simulated to evaluate whether the SIEM could detect and log abnormal activity.

---

## Project Goals

The primary goal of this capstone project was to design and evaluate a SIEM-based security monitoring system in a simulated small business environment.

### Objectives

- Build a safe, isolated virtual lab environment
- Deploy and configure a centralized Wazuh SIEM server
- Connect Windows and Linux endpoints to the SIEM
- Generate baseline user activity for normal behavior comparison
- Simulate suspicious and malicious actions on endpoints
- Analyze SIEM alerts and evaluate detection effectiveness
- Demonstrate how centralized monitoring supports security operations

---

## Lab Environment

### Virtual Infrastructure

- **Host Platform:** Ubuntu host system
- **Virtualization:** QEMU/KVM with `virt-manager`
- **Network Type:** NAT-based private virtual network (`siem-lab-net`)

### Systems Deployed

- **Wazuh Server:** Ubuntu Server
- **Endpoint 1:** Windows workstation
- **Endpoint 2:** Ubuntu Linux workstation

The Wazuh server acted as the centralized collection and analysis platform, while both endpoints forwarded logs and events using Wazuh agents.

---

## Tools and Technologies Used

- **Wazuh**
- **Ubuntu Server**
- **Windows**
- **Linux**
- **QEMU/KVM**
- **virt-manager**
- **NAT virtual networking**
- **MITRE ATT&CK Framework**

---

## Simulated Security Events

The following security-relevant activities were simulated and monitored:

- Repeated failed login attempts
- Successful login following repeated failures
- Firewall disablement and re-enablement
- User creation and administrator group changes
- File modifications in sensitive locations
- File permission and configuration changes

These simulations were used to test whether the SIEM could identify and surface suspicious behavior for analysis.

---

## Key Findings

The Wazuh SIEM successfully detected and logged multiple categories of suspicious activity across both Windows and Linux endpoints.

### Detection Areas Observed

- Authentication anomalies
- Privilege escalation behavior
- System configuration changes
- Account management activity
- File integrity and modification events

The project demonstrated that centralized log monitoring improves:

- visibility across systems
- detection of abnormal activity
- incident investigation capability
- overall security posture

Even in a small lab environment, the SIEM provided meaningful insights that support real-world SOC workflows.

---

## Example Incident Analysis

One simulated scenario involved repeated failed login attempts against a Windows endpoint, followed by a successful login.

This pattern resembled a brute-force authentication attempt. The SIEM generated authentication-related alerts that made it possible to identify:

- the affected endpoint
- the targeted account
- repeated failed attempts in a short timeframe
- evidence of a successful login after multiple failures

From a SOC perspective, this type of alert would support rapid investigation, account review, and response actions such as password resets, account lockout, or source validation.

---

## MITRE ATT&CK Relevance

Selected simulated behaviors were mapped to concepts from the **MITRE ATT&CK Framework** to demonstrate how SIEM alerts can support structured security analysis.

This helped connect lab activity to real-world attack techniques and provided a stronger incident analysis perspective.

---

## Skills Demonstrated

This project helped demonstrate hands-on experience with:

- SIEM deployment and configuration
- centralized log collection
- endpoint monitoring
- security event analysis
- alert investigation
- incident detection
- attack simulation
- security documentation
- MITRE ATT&CK mapping
- SOC-style workflow thinking

---

## Repository Structure

```text
siem-security-monitoring-lab/
├── README.md
├── docs/
│   ├── capstone-report.pdf
│   ├── capstone-proposal.pdf
│   └── topic-approval.pdf
├── screenshots/
│   ├── wazuh-dashboard-overview.png
│   ├── connected-agents.png
│   ├── brute-force-simulation.png
│   ├── firewall-change-alert.png
│   ├── user-creation-alert.png
│   └── linux-attack-events.png
├── diagrams/
│   └── siem-lab-network-diagram.png
├── writeups/
│   ├── lab-setup.md
│   ├── simulated-attacks.md
│   └── incident-analysis.md
└── resume-reference/
    └── project-summary.md
