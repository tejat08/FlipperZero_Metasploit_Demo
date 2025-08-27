# Kali VM Setup Guide for Flipper Zero + Metasploit Demo

This guide outlines how to set up the Kali Linux virtual machine used in the Flipper Zero + Metasploit reverse shell demo. This VM acts as the C2 (Command & Control) server hosting Metasploit, payloads, and a local HTTP server.

---

## 1. System Requirements

* **OS:** Kali Linux (Rolling Release)
* **RAM:** 4 GB minimum (8 GB recommended)
* **CPU:** 2 Cores minimum
* **Disk Space:** 40 GB minimum
* **Virtualization Tool:** VirtualBox or VMware
* **Network Mode:** Bridged Adapter

---

## 2. Download Kali ISO

* Official download link: [https://www.kali.org/get-kali/#kali-virtual-machines](https://www.kali.org/get-kali/#kali-platforms)
* Download the **VirtualBox** or **VMware** pre-built image for convenience.

---

## 3. Initial Setup (Importing Kali VM Image)

This section explains how to import and configure the prebuilt Kali Linux VM image for both **VirtualBox** and **VMware**.

---

### 🖥️ For VirtualBox:

#### ✅ Step 1: Install VirtualBox

If not already installed, download and install VirtualBox from:

👉 [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)

#### ✅ Step 2: Import the Kali `.ova` File

1. Open **VirtualBox**.
2. From the menu, click on **File** → **Import Appliance...**
3. Click **Choose** and browse to the downloaded `.ova` file (e.g., `kali-linux-2024.2-virtualbox-amd64.ova`).
4. Click **Next**, then **Import**.
5. Wait for the VM to be imported (this may take a few minutes).

#### ✅ Step 3: Configure the VM

After import:

1. Select the **Kali Linux VM** in the left panel.
2. Click **Settings**.
3. Navigate to the **System** tab:
   - Under **Processor**, set at least **2 cores** (more if available).
4. Navigate to the **Network** tab:
   - Under **Adapter 1**, check **Enable Network Adapter**.
   - In **Attached to**, select **Bridged Adapter** from the dropdown.
   - Leave other settings as default.

---

### 💻 For VMware:

#### ✅ Step 1: Install VMware Workstation Player

If you don’t have VMware:

👉 Download VMware Workstation Player (Free for personal use):  
[https://www.vmware.com/products/workstation-player.html](https://www.vmware.com/products/workstation-player.html)

#### ✅ Step 2: Open the Kali `.vmx` File

1. Extract the downloaded Kali VMware ZIP archive (e.g., `kali-linux-2024.2-vmware-amd64.zip`).
2. Inside the extracted folder, locate the file with `.vmx` extension (e.g., `kali-linux-2024.2-vmware-amd64.vmx`).
3. Open **VMware Workstation Player**.
4. Click **Open a Virtual Machine**.
5. Browse to and select the `.vmx` file.
6. Click **Finish** to add the VM.

#### ✅ Step 3: Configure the VM Settings

1. Select the **Kali VM** in VMware.
2. Click **Edit virtual machine settings**.
3. Under **Processors**, increase to **2 or more cores**.
4. Go to the **Network Adapter** tab:
   - Ensure **Bridged (direct connection to the physical network)** is selected.
   - Optionally, check **Replicate physical network connection state**.

---

## 4. Kali Configuration

After launching the VM:

### Update System:

```bash
sudo apt update && sudo apt upgrade -y
