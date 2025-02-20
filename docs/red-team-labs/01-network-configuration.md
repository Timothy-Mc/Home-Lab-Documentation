# Network Configuration in Proxmox

## Setting Up Open Switch and Linux Bridge
Before setting up **pfSense**, configure the network switch and add another adapter for **WAN** and **LAN**.

### Install Open vSwitch
Access **Proxmox** shell and run the following commands:
```sh
apt update
apt install openvswitch-switch
```

### Create a Linux Bridge for LAN
1. Click **Proxmox Server** → **System** → **Network** → **Create** → **Linux Bridge**.
2. Name the bridge and assign an IPv4/CIDR.
	- `vmbr0` (WAN)
	- `vmbr1` (LAN Lab)
3. Click **Create**.

### Create an OVS Bridge for the Switch
1. Click **Proxmox Server** → **System** → **Network** → **Create** → **OVS Bridge**.
2. Name the bridge and add a comment.
      - `vmbr2` (VLAN Interface)
3. Click **Create**.

### Create VLANs (vlan10, vlan20, vlan30)
1. Click **Create OVS IntPort**.
2. Create VLANs:
	- **vlan10** (VLAN Tag: 10) (OVS Bridge: vmbr2)
	- **vlan20** (VLAN Tag: 20) (OVS Bridge: vmbr2)
	- **vlan30** (VLAN Tag: 30) (OVS Bridge: vmbr2)
3. Click **Apply Configuration**.

---
## pfSense VM Setup
1. Click **Create VM**.
2. Name VM: `lab-pfsense`.
3. VM ID: `your choice`
4. Select storage and choose the downloaded pfSense ISO.
5. Click **Next** through the following sections:
	- System: **Default**
	- Disk Space: **200G**
	- Cores: **2**
	- Memory: **2G**
	- Bridge: **vmbr0** (default, second bridge to be added later)
6. Click **Finish**.

### Add Second Network Adapter
1. Click on **VM** → **Hardware** → **Add** → **Network Device**.
2. Select the **Linux Bridge** `vmbr1`.
3. Add another adapter for the **OVS Bridge** `vmbr2`.

---
## Installing and Configuring pfSense
1. Start the VM.
2. Open **Console** and start the installation.
3. Accept all defaults.
4. Select **Install pfSense**.
5. If prompted, choose **No** for manual partitioning.
6. Installation will proceed.

### Assign Interfaces
1. When prompted: `Should VLANs be set up now? [y,n]` → **n** (No)
2. Assign interfaces:
	- **WAN:** `vtnet0`
	- **LAN:** `vtnet1`
3. Confirm settings (`y` and hit Enter).

### Configure LAN
1. After installation, type `2` to access the LAN configuration.
2. Configure LAN settings.
	- Select LAN interface `2`
	- Configure IPv4 address LAN interface via DCHP `n`
	- `New LAN address here` → Example: `10.0.1.254/24`
	- Subnet Mask `24`
	- **ENTER**
	- Configure IPV4 address LAN interface via DCHP `n`
	- **ENTER**
	- Enable DHCP on LAN `y`
	- Address range of IPv4 client
		- `Start Range` → Example: `10.0.1.50`
        - `End Range` → Example: `10.0.1.100`
3. Revert to HTTP `n`, keep it HTTPS.

---
## Next Steps
In the next part, will be:
- [Configuring Kali Linux and completing pfSense setup](https://github.com/Timothy-Mc/Home-Lab-Documentation/blob/main/docs/red-team-labs/02-kali-pfsense-configuration.md)
