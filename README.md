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

---

### 🔹 Step 5 – Create GPO for WSUS Clients

On DC01:
1. Open Group Policy Management
2. Create a new GPO → WSUS_Client_Updates
3. Navigate to
```
Computer Configuration → Policies → Administrative Templates → Windows Components → Windows Update
```
4. Configure:
```
Specify intranet Microsoft update service location & set both as:
http://DC01:8530
```

<details> <summary>📸 Click to view screenshot of results</summary>
<p align="center">
  ✅ <strong>GPO WSUS Settings Applied</strong>✅  
<p align="center">
<img src="https://i.imgur.com/ZIw5RVZ.png" width="60%">
</p>
</details>

---

### 🔹 Step 6 – Apply GPO and Verify on Client01

On Client01:

```powershell
gpupdate /force
gpresult /r
```
Confirm your GPO (e.g., WSUS_Client_Updates) is listed under “Applied Group Policy Objects.”
Then force WSUS detection:
```powershell
wuauclt /detectnow
wuauclt /reportnow
```

<details> <summary>📸 Click to view screenshot of results</summary>
<p align="center">
  ✅ <strong>Client01 listed in WSUS Console</strong>✅  
<p align="center">
<img src="https://i.imgur.com/QSpGqkl.png" width="60%">
</p>
</details>

---

### 🔹 Step 7 – Approve and Deploy Updates

In the WSUS Console:
```
Updates → All Updates → Unapproved → Approve
```
- Choose your target group (or “All Computers”)
- Approve for Install

 <details> <summary>📸 Click to view screenshot of results</summary>
<p align="center">
  ✅ <strong>Approved Updates List</strong>✅  
<p align="center">
<img src="https://i.imgur.com/OUngoHM.png" width="60%">
</p>
</details> 

---

### 🔹 Step 8 – Verify Compliance

Check:
```
Reports → Computers → Computer Status Summary
```
✅ Client01 should appear and report its update status (Installed / Needed / Failed).

 <details> <summary>📸 Click to view screenshot of results</summary>
<p align="center">
  ✅ <strong>WSUS Compliance Report</strong>✅  
<p align="center">
<img src="https://i.imgur.com/XAlvAZZ.png" width="60%">
</p>
</details> 
