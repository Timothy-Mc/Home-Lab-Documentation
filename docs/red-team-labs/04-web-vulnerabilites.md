# Security Home Lab Setup

## Part 1: Setting Up Ubuntu Server, Docker, and Portainer

### Download Ubuntu Server

To begin, we will set up an **Ubuntu 24.04 live server**.

**Ubuntu 24.04.4 LTS:** [Website Download](https://ubuntu.com/download/server)

### Proxmox Setup

1. Assign a VM ID (e.g., `300` for the `30` subnet).
2. Name your machine and proceed.
3. Add your ISO and continue.
4. Allocate **160-200GB** storage.
5. Assign at least **2 CPU cores**.
6. Allocate **4GB RAM**.
7. Choose the **OVS bridge** and add the `30` tag.
8. Finish setup and start the machine.

### Installing Ubuntu Server

1. Choose your language.
2. Skip updates and update after installation.
3. Select keyboard language.
4. Leave network settings as default (subnet `30`).
5. Use entire disk for installation.
6. Confirm changes and proceed.
7. Install **OpenSSH Server**.
8. Complete installation and reboot.

After logging in, find your IP address:
```sh
ip a
```

### SSH into the Server from Kali Linux

```sh
ssh username@10.10.30.50 -p 22
```

### Updating the System

```sh
sudo apt update
```

### Installing Docker

Remove old Docker versions:
```sh
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

Add Docker’s official GPG key:
```sh
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

Add the repository to Apt sources:
```sh
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

Install Docker:
```sh
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Setting Up Portainer

Create a Docker volume:
```sh
docker volume create portainer_data
```

Run the Portainer container:
```sh
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```

Check running containers:
```sh
sudo docker ps
```
Open a browser and navigate to the **IP address** of your server (`https://<your-ip>:9443`). Click **Advanced > Accept the Risk and Continue**.
Create a **username and password** to complete setup.

---

## Part 2: Setting Up the Network and Deploying Containers

### Configuring the Network

1. On the left-hand side, click **Network**.
2. Name your network (e.g., `vlan30-config`).
3. In **Driver**, select **Macvlan**. A configuration section should appear.
4. Open a terminal and type:
   ```sh
   ifconfig
   ```
   This will show you the name of your network card. Copy the name.
5. Enter your network card name in the **Parent Network Card** field.
6. Below that, enter your **Subnet, IP Range, and Gateway**.
7. Scroll down and click **Create the Network**.
8. Confirm that your new network was created.

### Creating Another Network

1. Click **Network** and create a new network with the name `vlan30`.
2. Choose **Macvlan** again.
3. Instead of hitting **Configuration**, click **Creation**.
4. In the **Configuration** field, select the network you just created.
5. Click **Create the Network**.

## Creating Containers

### Deploying bWAPP Container

1. Click **Containers** in the left menu, then click **Create Container**.
2. Name it `lab-bwapp`.
3. In the **Image** field, click the **Search** button.
4. In the search bar, type `bwapp`, then select `raesene/bwapp`.
5. Copy the name and paste it into the **Image** field.
6. Scroll down to **Advanced Container Settings**.
7. Click the **Network** tab and select the VLAN you created.
8. Click **Deploy the Container**.
9. Once deployed, note the container’s IP address.
10. Open another browser tab and enter the IP followed by `/install.php`.
11. Click on the installation link and complete the setup.

### Deploying DVWA Container

1. Repeat the same steps as for `lab-bwapp`.
2. Name this container `lab-dvwa`.
3. Copy the DVWA image name and paste it into the **Image** field.
4. Assign it to the network and deploy it.
5. Once deployed, find the IP and open it in a browser.

### Deploying WebGoat Container

1. Create another container.
2. Name it `lab-webgoat`.
3. Paste the WebGoat Docker image name into the **Image** field.
4. Assign it to the network and deploy it.
5. Copy the container's IP and open another browser tab.
6. In the address bar, enter:

   ```
   http://<container-ip>/WebGoat/login
   ```

## Wrapping Up

We have successfully set up our Security Home Lab with:

- **Pfsense**
- **Kali Linux**
- **Active Directory**
- **Portainer** with vulnerable web applications