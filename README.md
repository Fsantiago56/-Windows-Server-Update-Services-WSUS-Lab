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
Install-WindowsFeature -Name UpdateServices, UpdateServices-DB, UpdateServices-RSAT, UpdateServices-UI -IncludeManagementTools
```
When prompted by the wizard:
- Choose WSUS Services and Database option.
- Select your content directory (e.g., D:\WSUS) — ensure at least 30 GB free space.
- Choose sync schedule: manual or automatic.


<details> <summary>📸 Click to view screenshot of results</summary>
<p align="center">
  ✅ <strong>WSUS Configuration Wizard summary screen </strong>✅  
<p align="center">
<img src="https://i.imgur.com/pWcfLZx.png" width="60%">
</p>
</details>
