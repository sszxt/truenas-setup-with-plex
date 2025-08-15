# TrueNAS Community Edition: Installation and Configuration Guide

This guide provides a comprehensive walkthrough for installing **TrueNAS Community Edition**, setting up a storage pool, creating datasets, managing users, and accessing your storage from a Windows machine via SMB.  
It also covers how to install and configure a **Plex Media Server**.

---

## Prerequisites

Before you begin, make sure you have the following:

- **TrueNAS Community Edition ISO**  
  Download from: [TrueNAS Community Edition Download](https://www.truenas.com/download-truenas-community-edition/)

- **Rufus (Version 4.0 or newer)**  
  Download from: [Rufus Official Website](https://rufus.ie/)

- **USB Flash Drive**  
  Minimum **8GB** capacity. (This drive will be erased during the process.)

- **A Dedicated System for TrueNAS**  
  - At least **8GB of RAM**  
  - One or more HDDs/SSDs for storage  
  - **Note:** The drive you install TrueNAS on cannot be used in your storage pool.

---

## 1️ Creating the Bootable USB Drive

1. **Download Files**  
   Ensure you have downloaded both the **TrueNAS ISO** and **Rufus**.

2. **Launch Rufus**  
   Open the Rufus `.exe` file.

3. **Configure Rufus**  
   - **Device:** Select your USB flash drive.  
   - **Boot selection:** Click **SELECT** and choose the TrueNAS `.iso` file.  
   - **IMPORTANT:** When prompted, select **DD Image mode**.

4. **Start the Process**  
   - Click **START**.  
   - Confirm that all data will be erased.  
   - Wait until Rufus finishes creating the bootable drive.

---

## 2 Installing TrueNAS

1. **Insert USB Drive** into the target computer.

2. **Boot from USB**  
   - Enter BIOS/UEFI (keys: `F2`, `F10`, `F12`, `DEL`).  
   - Set your USB drive as the boot device.

3. **Start Installation**  
   - Select **`1. Install/Upgrade`**.  
   - Highlight the drive where TrueNAS should be installed (preferably a separate SSD).  
   - Press **Spacebar** to select and then **Enter**.

4. **Confirm & Set Credentials**  
   - Select **Yes** to erase the installation drive.  
   - Set a **root password** (don’t forget this).

5. **Complete Installation**  
   - Remove the USB drive.  
   - Select **Reboot System**.

---

## 3️ Initial Access and Configuration

1. **Find Your IP Address**  
   - On the console, TrueNAS will show your system's IP (e.g., `http://192.168.1.150`).

2. **Log in to Web Interface**  
   - Open a browser on a computer in the same network.  
   - Go to the IP address.  
   - **Username:** `root`  
   - **Password:** (set during installation).

---

## 4️ Setting Up Storage

### Step 1: Create a Storage Pool
- Go to **Storage > Pools** → **ADD** → **Create new pool**.
- Name your pool (e.g., `main-storage`).
- Drag disks from **Available Disks** to **Data VDevs**.
- Choose a layout (e.g., `RAIDZ1` for redundancy).
- Click **CREATE**.

### Step 2: Create a Dataset
- Go to **Storage > Pools**.
- Next to your pool, click **⋮ > Add Dataset**.
- Name it (e.g., `documents` or `media`).
- Click **SAVE**.

### Step 3: Create a User Account
- Go to **Credentials > Local Users** → **ADD**.
- Fill in details, enable **Samba Authentication**.
- Click **SAVE**.

---

## 5️ Accessing Storage from Windows (SMB)

### Step 1: Create the SMB Share
- Go to **Sharing > Windows Shares (SMB)** → **ADD**.
- Path: Select dataset path (e.g., `/mnt/main-storage/documents`).
- Name your share.
- Enable SMB service.

### Step 2: Set Share Permissions
- Go to **Storage > Pools** → **⋮ > Edit Permissions**.
- Assign to your created user.
- Apply recursively.

### Step 3: Connect from Windows
- Open File Explorer → Right-click **This PC** → **Map network drive**.
- Example path: `\\192.168.1.150\Documents`
- Use your TrueNAS username and password.

---

## 6 Installing Plex Media Server

### Step 1: Create Datasets for Plex
- Create datasets:  
  - `media`   
  - `plex-config`   
  - `plex-transcode` 
  - `plex-storage`
  - `plex-data`

### Step 2: Install Plex
- Go to **Apps > Available Applications**.
- Search **Plex** → **Install**.
- Enable **Host Network**.

### Step 3: Configure Mount Points
- Go to **Apps > plex > Edit > Mount Points**.
- Add:
  - `/mnt/main-storage/media` → `/mnt/media`
  - `/mnt/main-storage/plex-config` → `/config`
  - `/mnt/main-storage/plex-transcode` → `/transcode`

### Step 4: Set Media Permissions for Plex
- Go to **Storage > Pools > media > Edit Permissions**.
- Add ACL item for user `plex` with **Read** access.

### Step 5: Access Plex
- Open: `http://YOUR_TRUENAS_IP:32400/web`
- Log in to Plex and add media from `/media`.

---

## Reference Videos
- [TrueNAS Installation & SMB Setup](https://www.youtube.com/watch?v=geb8tLYftlA)  
- [TrueNAS + Plex Setup](https://youtu.be/Q6LcRWib-v4?feature=shared)

---
