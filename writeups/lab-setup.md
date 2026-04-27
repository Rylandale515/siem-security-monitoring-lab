# SIEM Lab Setup

This lab was designed to simulate a small organizational network in order to evaluate centralized security monitoring using the Wazuh SIEM platform. The environment was built to mirror a realistic small business infrastructure while remaining fully isolated from external systems.

---

## Host System

- **OS:** Ubuntu (host)
- **Hypervisor:** QEMU/KVM
- **Interface:** virt-manager

QEMU/KVM was selected over alternatives such as VirtualBox because it integrates natively with the Linux kernel, providing better performance and stability for running multiple concurrent VMs on a Linux host system.

---

## Virtual Machines Deployed

| Role | OS | IP Address |
|------|----|------------|
| Wazuh SIEM Server | Ubuntu Server | 192.168.100.213 |
| Windows Endpoint | Windows (Wazuh Agent) | 192.168.100.x |
| Linux Endpoint | Ubuntu 22.04 (Wazuh Agent) | 192.168.100.128 |

---

## Network Configuration

All three virtual machines were connected through a NAT-based virtual network named:

```
siem-lab-net
```

This private network allowed communication between machines while preventing any interaction with external networks or the host's primary network interface. Network connectivity was verified by confirming virtual NIC settings and performing successful ping tests between all three machines.

---

## Wazuh Deployment

Wazuh was installed on the Ubuntu Server VM and served as the centralized SIEM platform for the lab.

### Components Deployed

- **Wazuh Manager** — core processing and rule engine
- **Wazuh Indexer** — log storage and search backend
- **Wazuh Dashboard** — web-based interface for alert review and analysis (accessed via browser at `192.168.100.213`)

### Agent Deployment

Wazuh agents were installed on both the Windows and Linux endpoint VMs. After installation, each agent was enrolled to the Wazuh Manager and log forwarding was confirmed via the Wazuh Dashboard, which showed both agents as active and reporting.

Agent connectivity was validated by verifying that logs from each endpoint appeared in the Wazuh Dashboard's agent overview panel.

---

## Baseline Establishment

Before any attack simulations were performed, a baseline of normal user behavior was recorded on both endpoints. Baseline activities included:

- Standard user login and logout
- Creating and editing files
- General system interaction (browsing directories, opening applications)
- Normal reboot cycles

This baseline allowed the SIEM to establish expected behavioral patterns against which simulated suspicious activity could be compared.

---

## Security Rationale

Isolating the lab environment via the `siem-lab-net` NAT network served multiple purposes:

- Prevented simulated attack activity from impacting external systems
- Kept the testing scope controlled and reproducible
- Ensured that all generated alerts originated only from lab activity

This design mirrors the approach used in real-world SOC lab environments and reflects security best practices for isolated testing infrastructure.

---

*This lab was built and operated in February 2026 as part of a WGU Cybersecurity and Information Assurance capstone project.*
