# 🖥️ VirtualBox Home Lab — Windows 11 + Kali (Splunk & Sysmon)

![VirtualBox](https://img.shields.io/badge/VirtualBox-7.2.2-blue?logo=virtualbox&logoColor=white)
![Windows 11](https://img.shields.io/badge/Windows-11-blue?logo=windows&logoColor=white)
![Kali Linux](https://img.shields.io/badge/Kali%20Linux-2025.2-blue?logo=kalilinux&logoColor=white)
![Splunk](https://img.shields.io/badge/Splunk-Lab%20Setup-orange?logo=splunk&logoColor=white)
![Sysmon](https://img.shields.io/badge/Sysmon-Configured-success)

---

## 📌 Project overview
This project demonstrates the design and implementation of an **isolated cybersecurity home lab** using VirtualBox, Windows 11, and Kali Linux.  

The lab was built to practice:
- Endpoint and network monitoring  
- Safe log ingestion into Splunk  
- Using Sysmon for detailed Windows telemetry  
- Controlled attacker simulations in a **sandboxed, non-internet-exposed environment**

All steps were fully documented and verified with screenshots.  
👉 Please see the [`/images`](./images) folder for all supporting screenshots.

---

## 🔑 Key highlights
- Verified installer integrity (SHA-256) before installation.  
- Built Windows 11 and Kali Linux VMs in VirtualBox.  
- Configured **Internal Network (`Myhome`)** with static IPs for secure, isolated communication.  
- Installed Splunk Enterprise and ingested Windows + Sysmon logs.  
- Used snapshots to create restore points and ensure safe experimentation.  
- Troubleshot virtualization, networking, and fullscreen issues.  
- Generated detection telemetry using safe, controlled simulations (malware steps redacted).  

---
## 📚 Lessons learned

* Always verify downloads with cryptographic checksums before running installers.
* Snapshots are essential — take them before any experiment that modifies system state.
* Internal (isolated) networking prevents accidental exposure of tests to the host or Internet.
* Splunk + Sysmon provides effective visibility for endpoint detection and investigation.
* Clear documentation and screenshots make laboratory work reproducible and easier to present to hiring managers.

---

## 🛠 Tools & technologies
- **Virtualization:** VirtualBox 7.2.2  
- **Guest OS:** Windows 11, Kali Linux 2025.2  
- **Log Management:** Splunk Enterprise  
- **Endpoint Telemetry:** Microsoft Sysmon  
- **Languages & Utilities:** PowerShell, Bash, 7-Zip, checksum verification tools  

---

## ⚙️ Setup process

### 1. Download & verify VirtualBox
- Downloaded VirtualBox installer from the official site.  
- Verified installer integrity using SHA-256 in PowerShell:

```powershell
Get-FileHash -Path .\VirtualBox-7.2.2-170484-Win.exe -Algorithm SHA256
````markdown
## ⚙️ Setup process

### 1. Download & verify VirtualBox
- Downloaded VirtualBox from the official site.  
- Verified installer integrity using SHA-256 in PowerShell:
```powershell
Get-FileHash -Path .\VirtualBox-7.2.2-170484-Win.exe -Algorithm SHA256
````

* Compared the computed checksum with the official `SHA256SUMS` file to ensure the binary was not tampered with.
* Installed VirtualBox and confirmed successful startup.

---

### 2. Windows 11 VM

* Created a Windows 11 ISO using Microsoft's Media Creation Tool.
* In VirtualBox: **New → Name: `VMTestWin11` → mount ISO → install**.
* Configured username/password and assigned CPU, RAM, and disk based on host capabilities.
* Installed VirtualBox Guest Additions (`VBoxWindowsAdditions-amd64.exe`) to enable fullscreen, mouse integration and better drivers.
* Resolved a startup issue by enabling VT-x/AMD-V in the host BIOS/UEFI.

---

### 3. Kali Linux VM

* Downloaded the official Kali VirtualBox release and extracted the archive (7-Zip).
* Imported/registered the `.vbox` file in VirtualBox and started the VM with default settings.
* Completed initial Kali setup and verified basic functionality.

---

### 4. Snapshots & safety

* Created clean snapshots for both Windows and Kali immediately after base installations.
* Purpose: provide reliable restore points before experiments and to quickly revert if needed.
* Always work from a snapshot when performing tests that modify system state.

---

### 5. Networking configuration (Internal Network `Myhome`)

* Set both VMs to **Internal Network** with the name `Myhome` (VirtualBox → Settings → Network).
* Internal Network isolates VM-to-VM traffic from the host and the Internet.
* Assigned static IPv4 addresses (Internal Network has no DHCP):

  * **Windows:** `192.168.20.10` / `255.255.255.0`
  * **Kali:** `192.168.20.11` / `255.255.255.0`
* Verification commands:

  * Windows: `ipconfig /all`
  * Linux: `ifconfig` or `ip addr show`
  * Cross-VM: `ping 192.168.20.11` (from Windows)

---

### 6. Splunk installation (Windows VM)

* Temporarily switched Windows VM to NAT/DHCP to download Splunk Enterprise.
* Installed Splunk via MSI and created a local admin account during setup.
* Accessed Splunk web UI at `http://localhost:8000` on the Windows VM.
* Added Windows Event Log inputs (Application, Security, System) via **Add Data**, and created an index (e.g., `endpoint`) for Sysmon/WEL events.

---

### 7. Sysmon installation (Windows VM)

* Downloaded Sysmon from Microsoft and obtained a curated `sysmonconfig.xml` from a community template.
* Installed Sysmon with the configuration to capture process, network and file activity:

```powershell
.\Sysmon64.exe -i sysmonconfig.xml
```

* Verified Sysmon was running and producing events in Event Viewer (Applications and Services Logs → Microsoft → Windows → Sysmon).
* Confirmed Sysmon events were being ingested into Splunk by checking the `endpoint` index.

---

### 8. Controlled testing (safety-first)

* Performed controlled simulations **only** inside the isolated internal network to generate telemetry for detection practice.
* For limited downloads, switched Windows VM to NAT briefly, then reverted to `Myhome` for testing.
* Disabled Windows Defender only within the VM and only during controlled tests.
* Generated reconnaissance/process/network activity from Kali and validated corresponding events in Splunk.
* **Safety note:** All exploit/malware payload details are intentionally redacted from public documentation. Tests were run only on VMs under my control with snapshots in place.

---

## 🐞 Troubleshooting & fixes

* **Virtualization disabled:** Enabled VT-x/AMD-V in BIOS/UEFI to run 64-bit guests.
* **Fullscreen/borderless issues:** Installed Guest Additions inside the Windows guest.
* **Networking issues:** Ensure both VMs use the same Internal Network name and static IPs (Internal Network does not provide DHCP).
* **Sysmon errors:** Re-ran installer with the configuration file and confirmed service registration in Services and Event Viewer.

---

## ✅ Results & outcomes

* Built a reproducible, isolated VirtualBox lab with Windows 11 and Kali Linux VMs.
* Ingested Windows Event Logs and enriched telemetry with Sysmon into Splunk (`endpoint` index).
* Validated detection workflows by producing and locating related event telemetry (searches by EventCode, dashboard checks).
* Used snapshots and isolation to ensure safe, reversible testing.
* Documented the entire process with screenshots for reproducibility and review (see `./images`).

---

## 📸 References & Images

All screenshots and supporting images are in the `./images` folder. Key captures include:

* `vb_checksum.png` — VirtualBox SHA-256 verification
* `vm_creation.png` — Windows VM creation
* `guest_additions.png` — Guest Additions install
* `kali_import.png` — Kali import/startup
* `snapshot_examples.png` — Snapshot screenshots
* `network_config.png` — Static IP setup
* `splunk_setup.png` — Splunk Add Data & index settings
* `sysmon_events.png` — Sysmon events in Event Viewer / Splunk

> Open the `images/` folder in this repo to view the screenshots.
