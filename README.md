# Home Lab Setup

## Overview
Welcome to the **Home Lab Setup** repository! This multi-step guide walks you through building a fully functional security and networking lab using **pfSense, Kali Linux, Active Directory, and web vulnerability testing tools**.

This lab is designed for **ethical hacking, penetration testing, and security research**. By following this guide, you will:
- Set up **pfSense** as a firewall and network segmenter.
- Install **Kali Linux** as an attack machine.
- Configure **Active Directory** for domain authentication.
- Deploy **vulnerable web applications** for security testing.

## Repository Structure
```
/home-lab-setup/
│── docs/
│   ├── red-team-labs/
│   │    ├── overview.md
│   │    ├── 01-network-configuration.md
│   │    ├── 02-kali-pfsense-configuration.mb
│   │    ├── 03-active-directory.md
│   │    └── 04-web-vulnerabilites.md
│   │
│   └── blue-team-labs/
│         ├──
│         ├──
│         ├──
│         └──
│   
│── .gitignore
└── README.md
```

## Prerequisites
Before starting, ensure you have the following:
- **Proxmox** or a virtualization platform (VMware, VirtualBox, etc.)
- **pfSense ISO**
- **Kali Linux ISO** 
- **Windows Server 2019 ISO** (for Active Directory setup)

## Quick Start
1. Clone this repository:
   ```sh
   git clone https://github.com/yourusername/home-lab-setup.git
   cd home-lab-setup
   ```
2. Follow each guide inside the **docs/** folder step by step:
   - Start with `01-network-configuration.md` to configure the network and install pfsense.
   - 
   - 
   - 


## Next Steps
After completing this lab, you can:
- Expand with more **vulnerable machines** (Metasploitable, DVWA, etc.).
- Implement **Intrusion Detection Systems (IDS)** like Suricata.

