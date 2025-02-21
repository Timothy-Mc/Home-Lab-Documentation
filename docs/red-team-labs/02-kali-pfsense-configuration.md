# Kali Linux Installation and pfSense Configuration

## Kali Linux Installation
Let's install Kali. I chose to go with **Kali Linux Purple** as it is designed for both blue team and purple team operations.  

The download for that is below:
[Kali Linux Downloads](https://www.kali.org/get-kali/#kali-installer-images)

After the ISO has been download, add it to **Proxmox** and create VM

### VM Configuration
1. Click **Create VM**.
2. Name VM: `lab-kali`.
3. VM ID: `your choice`
4. Select storage and choose the downloaded kali ISO.
5. Click **Next** through the following sections:
	- System: **Default**
	- Disk Space: **32G**
	- Cores: **4**
	- Memory: **8G**
	- Bridge: **vmbr1** (Linux Bridge)
1. Click **Finish**.

### Installation Process:
1. **Select Language & Country**.  
2. **Enter Username & Create Password**.  
3. **Set Timezone**.  
4. **Partitioning:**  
	- Choose **Guided - use entire disk** → Select disk → Confirm **Yes**.  
5. **Choose Desktop Environment** (*Gnome recommended*).  
6. **Install GRUB** → Select **Yes**.  
7. **Finalise:**  
	- Click **Continue** → Select disk → **Finish** → VM restarts.  
8. **Login** with your password.

---

## pfSense Configuration

### Verify Network Setup:
- Open **terminal** and check that kali is on the same subnet as the firewall.

### Access pfSense:
1. Open **browser** and type the pfsense IP. (`https://LAN-IP`)
2. A security warning will show, click **Advanced** → **Continue to Site**
3. **Login Credentials:**
    - **Username:** `Admin`
    - **Password:** `pfsense`
  
### Basic Setup:
1. **Hostname & Domain:** Change if needed.
2. **DNS Configuration:**
	- Uncheck **Override DNS**.
3. **WAN Settings:**  
	- Scroll to the bottom and uncheck **Block private networks**.
4. **LAN Settings:**
	- Keep default as it is pulled from the pfSense setup
5. **Set New Login Password**.
6. **Reload Configuration** → Click **Finish**.

---

## VLAN and DHCP Configuration

### VLAN Creation:
1. Click **Interfaces** → **Assignments**
2. Go to **VLANs** → Click **Add**.
3. Set up VLANs on the **parent interface** (**vtnet1**):
	- **VLAN10** → Tag `10`
	- **VLAN20** → Tag `20`
	- **VLAN30** → Tag `30`
4. Click **Save**, then **Apply Changes**.

### Assign VLANs:

1. **Go to** `Interfaces` → `Assignments`. 
2. **Click** `Add` for each VLAN you created.
3. **Enable and Configure VLAN Interfaces**:
	- **Go to** `Interfaces` → Select the VLAN (e.g., `OPT1`, `OPT2`, etc.).
	- **Rename** the interface to match the VLAN (e.g., `VLAN10`, `VLAN20`).
	- **Set IPv4 Configuration** to `Static IPv4`.
	- Assign a Static IP Address**:
		- **VLAN 10** → `X.X.10.254/24` → Example: `10.0.10.254/24`
		- **VLAN 20** → `X.X.20.254/24` → Example: `10.0.20.254/24`
		- **VLAN 30** → `X.X.30.254/24` → Example: `10.0.30.254/24`
4. **Click Save**, then **Apply Changes**.
### Firewall Rules:
1. Go to **Firewall** → **Rules**
2. Select **VLAN10** → Click **Add**
3. **Rules:**
	- **Action:** Pass
	- **Interface:** VLAN10
	- **Address Family:** IPv4
	- **Protocol:** Any
	- **Source:** VLAN10 subnets
	- **Destination:** Any
	- **Description:** Allow VLAN10 to any
4. Add similar rules for **VLAN20** and **VLAN30**
5. Click **Apply Changes**

### DHCP Configuration:
1. Go to **Services** → **DHCP Server**
2. Configure **LAN DNS**
	- **LAN Subnet:** `Subnet of LAN` → Example: `10.0.1.0/24`
	- **Primary DNS:** `LAN Address` → Example: `10.0.1.254`
	- **Secondary DNS:** `8.8.8.8` Google DNS
	- Click **Save**
3. Configure **VLAN DHCP:**
	- **VLAN10**
		- **Subnet:** `X.X.10.0/24` → Example: `10.0.10.0/24`
		- **Range:**  `X.X.10.50` - `X.X.10.100` → Example: `10.0.10.50` - `10.0.10.100`
		- **DNS Server:** `X.X.10.254` → Example: `10.0.10.254`
		- **DNS Server 2:** `8.8.8.8` (Google DNS)
		- Click **Save**
	- **VLAN20**
		- **Subnet:** `X.X.20.0/24` → Example: `10.0.20.0/24`
		- **Range:**  `X.X.20.50` - `X.X.20.100` → Example: `10.0.20.50` - `10.0.20.100`
		- Click **Save**
	- **VLAN30**
		- **Subnet:** `X.X.30.0/24` → Example: `10.0.30.0/24`
		- **Range:**  `X.X.30.50` - `X.X.30.100` → Example: `10.0.30.50` - `10.0.30.100`
		- Click **Save**
4. Click **Apply Changes** 

---
## Next Steps:
In the next part, will be:
- [Building an Active Directory](03-active-directory.md)
