# 🧩 Windows Server Update Services (WSUS) Lab

## 🎯 Goal
Learn how to centrally manage, approve, and deploy Windows updates across a domain environment.

---

## 🖥️ Environments Used
- Host OS: Windows 11
- Virtualization: VirtualBox
- Server VM: Windows Server 2022 (Domain Controller or Standalone WSUS Server)
- Client VM: Windows 10 / 11 (domain-joined)

---

## 🛠️ Lab Walkthrough

### 🔹 Step 1 – Install WSUS Role

1. On DC01 or a dedicated server VM:

```powershell
# Open PowerShell as Administrator
Install-WindowsFeature -Name UpdateServices-Services, UpdateServices-WidDB -IncludeManagementTools
```
✅ Successfully installed using Windows Internal Database (WID) — avoids SQL conflicts.

---

### 🔹 Step 2 – Run WSUS Post-Install


```powershell
# Open PowerShell as Administrator
cd "C:\Program Files\Update Services\Tools"
.\wsusutil.exe postinstall CONTENT_DIR=C:\WSUS
```

<details> <summary>📸 Click to view screenshot of results</summary>
<p align="center">
  ✅ <strong> WSUS Post-Installation Completed Successfully </strong>✅  
<p align="center">
<img src="https://i.imgur.com/HUcR3QC.png" width="60%">
</p>
</details>

---

### 🔹 Step 3 – Launch WSUS Console

- Open Server Manager → Tools → Windows Server Update Services
- The Configuration Wizard will start automatically.
  Select:
- Upstream Source: Microsoft Update
- Languages: English
- Products: Windows 
- Sync Schedule: Manual


<details> <summary>📸 Click to view screenshot of results</summary>
<p align="center">
  ✅ <strong>WSUS Configuration Wizard Summary Screen </strong>✅  
<p align="center">
<img src="https://i.imgur.com/HSmTxUN.png" width="60%">
</p>
</details>

---

### 🔹 Step 4 – Synchronize Updates

From the WSUS console:
- Right-click Synchronizations → Start Synchronization

<details> <summary>📸 Click to view screenshot of results</summary>
<p align="center">
  ✅ <strong>Sync Progress Showing Successfully Completed</strong>✅  
<p align="center">
<img src="https://i.imgur.com/ZDEL0eK.png" width="60%">
</p>
</details>
