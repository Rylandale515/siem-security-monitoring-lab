# SIEM Lab Setup

This lab was designed to simulate a small organizational network in order to evaluate centralized security monitoring using the Wazuh SIEM platform.

## Virtual Infrastructure

The lab was built using QEMU/KVM virtualization managed through the virt-manager interface on an Ubuntu host system.

## Systems Deployed

The lab environment contained three virtual machines:

• Ubuntu Server running the Wazuh SIEM platform  
• Windows endpoint with Wazuh agent installed  
• Linux endpoint with Wazuh agent installed  

## Network Configuration

All systems were connected through a NAT-based virtual network named:

siem-lab-net

This allowed communication between machines while preventing interaction with external networks.

## SIEM Components

The Wazuh deployment included:

• Wazuh Manager  
• Wazuh Indexer  
• Wazuh Dashboard  

Agents installed on both endpoints forwarded logs to the SIEM server for centralized analysis.
