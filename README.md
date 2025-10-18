# 🖥️ VirtualBox Home Lab — Windows 11 + Kali Linux + Metasploitable

![VirtualBox](https://img.shields.io/badge/VirtualBox-7.2.2-blue?logo=virtualbox\&logoColor=white)
![Windows 11](https://img.shields.io/badge/Windows-11-blue?logo=windows\&logoColor=white)
![Kali Linux](https://img.shields.io/badge/Kali%20Linux-2025.2-blue?logo=kalilinux\&logoColor=white)
![Metasploitable](https://img.shields.io/badge/Metasploitable-Lab%20VM-orange)

---

## 📌 Project overview

This repository documents an **isolated cybersecurity home lab** built with VirtualBox to practice hands-on defensive and offensive techniques in a safe, sandboxed environment.

**Goal:** Build an isolated lab for testing and learning (no external exposure).
**Showcase:** Documented setup, VM configurations, NAT networking for the lab, and connectivity verification.

All supporting screenshots are in the `/images` folder.

<img src="https://github.com/nabirudd/virtualbox_home_lab/blob/main/Metasploit/Label%202.png?raw=true" alt="Setup" height="500" hspace="70">

---

## 🔑 Key highlights

* Built reproducible VirtualBox lab with **Windows 11**, **Kali Linux**, and **Metasploitable** VMs.
* Created a dedicated **NAT network** for the whole lab to isolate traffic from the host and wider Internet.
* Verified connectivity between VMs and documented network configuration and troubleshooting steps.
* Used snapshots for safe experimentation and quick rollback.

---

## 🛠 Tools & technologies

* **Virtualization:** VirtualBox 7.x
* **Guest OS:** Windows 11, Kali Linux (official VirtualBox build), Metasploitable (vulnerable training VM)
* **Utilities:** 7-Zip, checksum verification tools, PowerShell, Bash
* **Networking:** VirtualBox NAT Network (dedicated for lab)

---

## ⚙️ Setup process

### 1. Download & verify VirtualBox

* Download VirtualBox from the official site and verify the installer with SHA-256 before running.

```powershell
Get-FileHash -Path .\VirtualBox-7.2.2-170484-Win.exe -Algorithm SHA256
```

*Compare the computed checksum with the official SHA256SUMS provided by the VirtualBox site.*

<img src="https://github.com/nabirudd/virtualbox_home_lab/blob/main/1.%20Installation%20&%20verification%20Virtual%20Envriroment/2.%20Checking%20Hash%20On%20CMD.png?raw=true" alt="Setup" height="500" hspace="70">
<img src="https://github.com/nabirudd/virtualbox_home_lab/blob/main/1.%20Installation%20&%20verification%20Virtual%20Envriroment/1.%20Checking%20Hashing%20Code%20in%20Virtual%20Box%20Site.png?raw=true" alt="Setup" height="500" hspace="70">


---

### 2. Windows 11 VM

* Create a new VM in VirtualBox: **New → Name: `Win11-Lab` → Type: Microsoft Windows → Version: Windows 11 (64-bit)**.
* Attach the Windows 11 ISO, allocate CPU/RAM/disk according to host capacity, and proceed with installation.
* Install VirtualBox Guest Additions in the guest to enable improved drivers, mouse integration, and full-screen support.

<img src="https://github.com/nabirudd/virtualbox_home_lab/blob/main/Metasploit/windows%2011%20UI.png?raw=true" alt="Setup" height="500" hspace="70">
<img src="https://github.com/nabirudd/virtualbox_home_lab/blob/main/2.%20Create%20Windows%2011%20VM/1.%20Windwos%20Guest%20Edition%20Files.png?raw=true" alt="Setup" height="500" hspace="70">

---

### 3. Kali Linux VM

* Download the official Kali VirtualBox image or ISO and import/register the `.vbox` or use the installer.
* Create a Kali VM (or import the provided VirtualBox appliance) and complete initial setup.
* Verify basic functionality (networking, terminal, package updates).
<img src="https://github.com/nabirudd/virtualbox_home_lab/blob/main/Metasploit/Linux%20UI.png?raw=true" alt="Setup" height="500" hspace="70">

---

### 4. Metasploitable VM

* Download the Metasploitable VM (e.g., from SourceForge or the official archive) and extract the archive.
* Create a new VM in VirtualBox (select **Other Linux (64-bit)** if no direct match) and attach the Metasploitable image/ISO.
* Boot the VM and confirm it runs correctly.

<img src="https://github.com/nabirudd/virtualbox_home_lab/blob/main/Metasploit/Metasploit%20UI.png?raw=true" alt="Setup" height="500" hspace="70">

---

### 5. Create a dedicated NAT Network for the lab

* In VirtualBox Manager: **File → Host Network Manager** (or VirtualBox Network settings) → create a new **NAT Network** (example name: `LabNAT`).
* Configure the NAT Network as needed (DHCP on/off). A single NAT Network keeps outbound connectivity controlled while isolating VM traffic from other host networks.

<img src="https://github.com/nabirudd/virtualbox_home_lab/blob/main/Metasploit/NAT%20Network.png?raw=true" alt="Setup" height="500" hspace="70">

---

### 6. Connect VMs to the NAT Network

* For each VM (Windows 11, Kali, Metasploitable):

  * VirtualBox → **Settings → Network → Adapter 1** → Attached to: **NAT Network** → Name: `LabNAT`.
* Optionally add a second adapter (Host-only or Internal) if you need host ↔ VM access while keeping outward traffic controlled. For pure isolation keep all lab VMs on the same NAT Network.

<img src="https://github.com/nabirudd/virtualbox_home_lab/blob/main/Metasploit/Meta%20Network%20Setup.png?raw=true" alt="Setup" height="500" hspace="70">

---

### 7. IP addressing & verification

* If DHCP is enabled on the NAT Network, VMs will receive addresses automatically. If you prefer static addressing, configure static IPv4 addresses inside each guest.
* Commands to verify network configuration and connectivity:

Windows (inside VM):

```powershell
ipconfig /all
ping <other-vm-ip>
```

Linux (Kali / Metasploitable):

```bash
ip addr show
ping <other-vm-ip>
```

<img src="https://github.com/nabirudd/virtualbox_home_lab/blob/main/Metasploit/Ip%20Testing.png?raw=true" alt="Setup" height="500" hspace="70">


---

### 8. Snapshots & safety

* Create clean snapshots immediately after installing and configuring each VM:

  * VirtualBox Manager → Select VM → Snapshots → Take Snapshot.
* Purpose: quick rollback after experiments and to maintain a known good baseline.

---

## 🐞 Troubleshooting & fixes

* **NAT network issues:** Ensure all VMs are attached to the same NAT Network name and that VirtualBox’s NAT Network service is running. If DHCP is disabled, assign static IPs inside each guest.
* **VM performance problems:** Adjust CPU/RAM allocation or enable VT-x/AMD-V in the host BIOS/UEFI.
* **Fullscreen/mouse integration issues:** Install/repair Guest Additions in the guest OS.
* **Metasploitable unavailable/boot errors:** Verify the image was imported correctly and file integrity (re-extract archive if necessary).

---

## 📚 Lessons learned

* Use snapshots liberally before making changes — they save time and prevent data loss.
* A dedicated NAT Network allows controlled Internet access (if desired) while keeping lab traffic isolated from the host and external networks.
* Keep vulnerable targets (Metasploitable) strictly inside lab networks — never expose them to your home/office network.
* Clear documentation (commands, IPs, screenshots) makes the lab reproducible and easy to present to hiring managers.

---

## ✅ Results & outcomes

* Reproducible VirtualBox lab containing Windows 11, Kali Linux, and Metasploitable.
* Dedicated NAT Network (`LabNAT`) connecting all lab machines and isolating experiments.
* Verified inter-VM connectivity and documented setup for portfolio presentation.
* Full set of screenshots and configuration artifacts saved to `./images` for review.

---

