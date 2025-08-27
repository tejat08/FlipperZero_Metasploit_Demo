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

## 3. Initial Setup
🖥️ For VirtualBox:
✅ Step 1: Install VirtualBox

If not already installed, download and install VirtualBox from:

👉 https://www.virtualbox.org/wiki/Downloads

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
