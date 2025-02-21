# Building an Active Directory

## Part 1: Windows Server Setup

### Windows ISO Downloads

- **Windows Server 2022**: Have to find download.
- **Windows 10**: Have to find download.
- **VirtIO Drivers**: [GitHub - VirtIO-Win](https://github.com/virtio-win/virtio-win-pkg-scripts) 

---

## Windows Server Installation on Proxmox

Setting up a Windows ISO on Proxmox requires additional configuration. Follow these steps:

### Windows Server VM Configuration

1. **Name & ID**    
    - Name your machine (e.g., `lab-dc`)        
    - Assign an ID (e.g., `200` for the `20` subnet)        
2. **OS Setup**    
    - Select the storage device where the Windows Server ISO is located        
    - Add the ISO        
    - Choose **Microsoft Windows** as the OS type        
    - Check **Add additional drive for VirtIO drivers**        
    - Select the storage for VirtIO ISO and add it        
3. **System Configuration**    
    - Set **Machine** to `q35`        
    - Check **Add TPM**        
    - Enable **Qemu Agent**        
    - Ensure **BIOS** is set to `OVMF UEFI`        
4. **Disk Setup**    
    - Set `Bus/Device` to **VirtIO Block**        
    - Allocate at least `64GB`        
    - In `Cache`, select **Write Back**        
5. **CPU Configuration**    
    - Assign `2 cores`        
    - Set **Type** to `Host`        
6. **Memory Configuration**    
    - Allocate `4GB` RAM (default)        
7. **Network Configuration**    
    - Add the **OVS bridge**        
    - Set the **VLAN Tag** to `20`        
8. **Finalize Setup**    
    - Click **Finish**        

### Adding VirtIO ISO (If Not Added Initially)
1. Click on the machine in Proxmox    
2. Navigate to `Hardware` → `Add`    
3. Select **CD/DVD drive**    
4. Choose the VirtIO ISO storage and click **Add**    
5. Start the machine and press `Enter` if needed    

---

## Windows Installation

1. Click **Next** → **Install Now**    
2. Select **Desktop Experience** → **Next**    
3. Accept License Terms → **Next**    
4. Choose **Custom Installation**    

### Loading VirtIO Drivers

1. Click **Load Driver** → **Browse**    
2. Select the VirtIO ISO drive    
3. Navigate to `amd64` → `2k22` and click **OK**    
4. Select the displayed driver → **Next**    
5. Select the disk → **Next** (Installation begins)    

---

## Post-Installation Configuration

### Setting Up Network

1. Open PowerShell and check IP configuration:    
    ```
    ipconfig
    ```
    
2. Set a **Static IP**:    
    - Open **Network & Internet Settings**        
    - Click **Change adapter options**        
    - Right-click **Ethernet** → **Properties**        
    - Select **Internet Protocol Version 4 (TCP/IPv4)**        
    - Choose **Use the following IP address** 
    - Set **DNS Server** (local and Google DNS)
        

### Renaming the Computer

1. Open **File Explorer** → **This PC** → **Properties**
2. Click **Rename this PC** 
3. Set a name (e.g., `lab-dc`) 
4. Restart the machine
    
---

## Setting Up Domain Controller, DNS, and DHCP

### Installing Roles

1. Open **Server Manager**
2. Click **Manage** → **Add Roles and Features**
3. Select:
    - **Active Directory Domain Services**   
    - **DHCP Server**   
    - **DNS Server**  
4. Click **Install** and restart

### Promoting to Domain Controller

1. Open **Server Manager**
2. Click **Yellow Warning Flag** → **Promote this server to a domain controller** 
3. Select **Add a new forest**
4. Set domain name (`example.local`) 
5. Enter a **password** 
6. Click **Install** and restart 

---

## Configuring DHCP Server

### Disabling DHCP on pfSense

1. Log in to **pfSense WebUI** 
2. Navigate to **Services** → **DHCP Server** 
3. Select **VLAN20** 
4. Uncheck **Enable DHCP Server on VLAN20**  

### Setting Up DHCP on Windows Server

1. Open **DHCP Manager**
2. Expand **IPv4** → **New Scope**
3. Name it `VLAN20`
4. Set **Start IP**, **End IP**, and **Subnet Mask** 
5. Add **Default Gateway**
6. Click **Next** until finish
7. Complete **DHCP Server Configuration** in **Server Manager**
8. Click **Commit** and **Close* 

---

## Part 2: Windows 10 VM Setup and Domain Join

### Creating Windows 10 VM in Proxmox

1. Assign an ID (e.g., `201` for VLAN `20`)
2. Select **Windows 10 ISO**
3. Check **Qemu Agent**
4. Enable **Add TPM**
5. Set **Machine** to `q35`
6. Set **BIOS** to `OVMF UEFI`

### Disk & CPU Configuration

1. Set `Bus/Device` to **VirtIO Block**
2. Allocate at least `32GB` (recommended: `64GB`)
3. Set **Cache** to **Write Back**
4. Assign `2 CPU cores`
5. Allocate `4GB` RAM
    

### Network Configuration

1. Select **OVS bridge* 
2. Add **VLAN Tag 20* 

---

## Windows 10 Installation

1. Click **Next** → **Custom Installation**
2. Load **VirtIO Driver**:
    - Click **Load driver** → **Browse**    
    - Select **amd64** → **w10** → **OK**    
3. Click **Install**

### Initial Setup

1. Choose **Country** → **Yes**
2. Choose **Keyboard Layout** → **Yes**
3. Select **Domain Join Instead**
4. Create **Username** and **Password**
5. Disable all privacy settings
6. Click **Not Now**

---

## Joining Windows 10 to Domain

1. Open **File Explorer** → **This PC** → **Properties**
2. Click **Advanced System Settings** → **Computer Name**
3. Click **Change** → Enter Domain (`example.local`)
4. Use **Admin account** credentials 
5. Restart the machine  

### Verify Domain Connection

1. Lock computer (`Win + L`)
2. Select **Other User**
3. Log in using:
    ```
    example\username
    ``` 
4. Enter password

---

## Next Steps

In the next part, will be:
- [Vulnerable Web Applications](04-web-vulnerabilites.md).
