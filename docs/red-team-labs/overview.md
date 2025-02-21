# Red Team Labs Overview

This guide will walk you through setting up a **Red Team-oriented home lab** with the following components:

## Lab Components
- **pfSense** – Firewall for network segmentation and security.
- **Kali Linux** – Attack machine for penetration testing and exploitation.
- **Active Directory** – Windows Domain Controller and Windows 10 Workstation for testing privilege escalation and lateral movement.
- **Web Exploitation Targets**:
  - **Damn Vulnerable Web Application (DVWA)** – Test SQL injection, XSS, CSRF, and more.
  - **Boogie Web App** – Practice various web security vulnerabilities.
  - **WebGoat** – An interactive platform to learn and test web vulnerabilities.

## Guides Included
- [01-network-configuration.md](01-network-configuration.md) – Initial network setup and segmentation.
- [02-kali-pfsense-configuration.md](02-kali-pfsense-configuration.md) – Configuring pfSense with Kali Linux for network access and filtering.
- [03-active-directory](03-active-directory.md) - Deploying an Active Directory domain on Proxmox with Windows Server, network configuration, and domain integration.
- [04-web-vulnerabilites.md](04-web-vulnerabilites.md) - Setting up a Security Home Lab with Ubuntu Server, Docker, and Portainer for deploying vulnerable web applications like bWAPP, DVWA, and WebGoat.

Additional lab configurations and penetration testing methodologies will be added overtime.