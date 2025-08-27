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

* Official download link: [https://www.kali.org/get-kali/#kali-virtual-machines](https://www.kali.org/get-kali/#kali-virtual-machines)
* Download the **VirtualBox** or **VMware** pre-built image for convenience.

---

## 3. Initial Setup

### For VirtualBox:

1. Import the downloaded `.ova` file.
2. Configure the VM:

   * System > Processor: Set at least 2 cores.
   * Network > Adapter 1: Select **Bridged Adapter** to ensure it’s on the same network as the Windows VM and Flipper.

### For VMware:

1. Open VMware Workstation/Player.
2. Use "Open a Virtual Machine" and select the `.vmx` file.
3. Ensure the network is set to **Bridged Mode**.

---

## 4. Kali Configuration

After launching the VM:

### Update System:

```bash
sudo apt update && sudo apt upgrade -y
```

### Install Dependencies:

```bash
sudo apt install metasploit-framework python3 python3-pip git -y
```

### Verify Metasploit Installation:

```bash
msfconsole --version
```

Should return something like: `Framework Version: 6.x.x`

---

## 5. Networking Checks

Ensure Kali and the Windows VM are on the same subnet.

Check your IP:

```bash
ip a
```

Ensure it shows something like `192.168.178.xxx`.

Ping the Windows VM:

```bash
ping <windows_vm_ip>
```

---

## 6. Enable Web Server for Payload Hosting

In your project folder (e.g., `~/flipper_msf_demo/web_payload/`):

```bash
cd ~/flipper_msf_demo/web_payload
python3 -m http.server 8081
```

This hosts your payload on `http://<kali_ip>:8081/payload.exe`

---

## 7. Optional: Add Flipper Tools

Install qFlipper:

```bash
chmod +x ./qFlipper-x86_64-*.AppImage
./qFlipper-x86_64-*.AppImage
```

---

## 8. Notes

* Always match `LHOST` with Kali's current IP.
* Disable any firewall if it blocks reverse shells.
* Ensure that Flipper and both VMs are connected to the same AP or LAN network.

---

## ✅ Kali VM Setup Complete!

Proceed to the next guide: `windows_vm_setup.md`.
