# 🧩 Active Directory & Windows Server Labs

> Simulating a **real-world small business IT infrastructure** — from Domain Controller setup to Group Policy enforcement and client domain joining.

<div align="center">

![Status](https://img.shields.io/badge/Status-In%20Progress-yellow?style=flat-square)
![Platform](https://img.shields.io/badge/Platform-Windows%20Server%202025-blue?style=flat-square&logo=windows)
![Virtualization](https://img.shields.io/badge/VMware-Workstation%20Pro-607078?style=flat-square&logo=vmware)
![Host](https://img.shields.io/badge/Host-Ubuntu-E95420?style=flat-square&logo=ubuntu)

</div>

---

## 📌 Overview

This repository documents hands-on labs for deploying and managing a **Windows Server Active Directory (AD) environment**, where an IT administrator is responsible for domain setup, user management, Group Policy configuration, client onboarding, and system troubleshooting.

---

## 🎯 Objectives & 🛠️ Technologies

<table>
<tr>
<td width="50%" valign="top">

**Objectives**

- 🏗️ Build a fully functional **Active Directory environment**
- 🌐 Understand **domain-based network architecture**
- 👥 Practice **user and access management**
- 🔒 Implement **security policies via Group Policy**
- 🧰 Gain real-world **IT Support / SysAdmin experience**

</td>
<td width="50%" valign="top">

**Technologies**

- 🪟 Windows Server 2025
- 💻 Windows 10 (Client Machine)
- 🗂️ Active Directory Domain Services (AD DS)
- 📋 Group Policy Management
- ⚙️ VMware Workstation Pro
- 🐧 Ubuntu (Host Machine)

</td>
</tr>
</table>

---

## 🧪 Lab Structure

| #   | Lab                                                               | Status       |
| --- | ------------------------------------------------------------------| ------------ |
| 1   | Windows Server Installation                                       | ✅ Completed |
| 2   | Active Directory Domain Controller Setup                          | ✅ Completed |
| 3   | User & Organizational Unit (OU) Management                        | ✅ Completed |
| 4   | Active Directory Groups – Security vs Distribution & Adding Users | ✅ Completed |
| 5   | Domain Joining (Windows 10 Client)                                | ⏳ Pending   |
| 6   | DNS & Networking Configuration                                    | ⏳ Pending   |
| 7   | Troubleshooting Scenarios                                         | ⏳ Pending   |
| 8   | Security Hardening & Best Practices                               | ⏳ Pending   |

---

# 🚀 Lab 1 — Windows Server Installation

## 🖥️ Environment & VM Configuration

<table>
<tr>
<td width="50%" valign="top">

**Environment**

- **Host OS:** Ubuntu
- **Virtualization:** VMware Workstation Pro
- **Guest OS:** Windows Server 2025

</td>
<td width="50%" valign="top">

**Recommended VM Specs**

- **RAM:** 2 GB min _(4 GB recommended)_
- **Storage:** 60 GB minimum
- **CPU:** 2 cores
- **Network:** NAT _(for internet access)_

</td>
</tr>
</table>

---

## 📋 Setup Steps

**Step 1 — Install VMware Workstation Pro**
Download and install VMware Workstation Pro on your Ubuntu host machine.

**Step 2 — Download Windows Server ISO**
Grab the **Windows Server 2025 ISO** from the [Microsoft Evaluation Center](https://www.microsoft.com/en-us/evalcenter/).

**Step 3 — Create a New Virtual Machine**
Create a new VM in VMware using the recommended configuration above.

**Step 4 — Install Windows Server**
Attach the ISO → Power on the VM → Follow the installation wizard → Select **Standard with Desktop Experience** → Complete setup and log in.

---

## ✅ Outcome

- Successfully installed **Windows Server 2025** on VMware Workstation
- Virtual machine is fully operational and accessible
- Environment is ready for **Active Directory configuration** _(Lab 2)_

---

## 📸 Screenshots

<p align="center">
  <img src="images/lab1/image.png" width="45%" />
  <img src="images/lab1/image-2.png" width="45%" />
</p>

---

# 🚀 Lab 2 — Active Directory Domain Controller Setup
 
## 🖥️ What Was Configured
 
<table>
<tr>
<td width="33%" valign="top">
 
**🖊️ Renamed Windows 11 PC**
- Renamed the client machine to a meaningful hostname before joining it to the domain
- Requires a **restart** to apply
 
</td>
<td width="33%" valign="top">
 
**🌐 Static IP Address**
- Assigned a **manual static IPv4 address** to the server so it is always reachable at a fixed address
- Required for reliable DNS and AD DS operation
 
</td>
<td width="33%" valign="top">
 
**🗂️ Installed & Promoted AD DS**
- Installed the **AD DS role** via Server Manager
- Promoted the server to a **Domain Controller**
- Created a new **forest and root domain**
 
</td>
</tr>
<tr>
<td width="50%" valign="top">
 
**📡 Pinged Windows Server 2025**
- Verified **network connectivity** between the Windows 11 client and the Windows Server 2025 VM
- Confirmed the static IP was reachable before attempting domain join
 
</td>
<td width="50%" valign="top">
 
**🔗 Joined Windows 11 PC to the Domain**
- Joined the Windows 11 client machine to the **Active Directory domain**
- Verified successful domain membership via **System Properties**
 
</td>
</tr>
</table>
 
---
 
## 📋 Setup Steps
 
**Step 1 — Rename the Windows 11 PC**
Open **Settings → System → About → Rename this PC** → Enter a descriptive hostname (`Workstation01`) → Restart to apply.
 
**Step 2 — Configure a Static IP Address on the Server**
Go to **Network Adapter Settings → IPv4 Properties** and set:
 
 ## Windows Server 2025 Static IP Configuration 

| Field | Value |
|-------|--------------|
| IP Address | `192.168.1.10` |
| Subnet Mask | `255.255.255.0` |
| Default Gateway | `192.168.1.1` |
| Preferred DNS | `127.0.0.1` *(points to itself after AD install)* |
 

## Client Machine ( Windows 11) Static IP Configuration

| Field | Value |
|-------|--------------|
| IP Address | `192.168.1.11` |
| Subnet Mask | `255.255.255.0` |
| Default Gateway | `192.168.1.1` |
| Preferred DNS | `192.168.1.10` *(points to Windows Server)* |
| Alternate DNS | `192.168.1.1` *(points to any other server on the IP gateway)* |



**Step 3 — Install the AD DS **
Open **Server Manager → Add Roles and Features** → Select **Active Directory Domain Services** → Proceed through the wizard and install.
 
**Step 4 — Promote Server to Domain Controller**
After installation, click **Promote this server to a domain controller** in Server Manager → Select **Add a new forest** → Enter your **Root Domain Name** (`InfoTech.com`) → Set a DSRM password → Complete the wizard → Server will **restart automatically**.
 
**Step 5 — Ping the Windows Server 2025**
On the Windows 11 client, open **Command Prompt** and run:
```
ping 192.168.1.10
```
Confirm you receive replies — this verifies network connectivity between the client and the server before proceeding with domain join.
 
**Step 6 — Join the Windows 11 PC to the Domain**
On the Windows 11 client, go to **Settings → System → About → Advanced system settings → Computer Name → Change** → Select **Domain** → Enter your domain name (`InfoTech.com`) → Provide **Domain Admin credentials** when prompted → Restart to complete.
 
---
 
## ✅ Outcome
 
- Windows 11 client machine successfully **renamed** to a meaningful hostname
- **Static IP** configured — server is reachable at a fixed network address
- **AD DS role** installed and server promoted to **Domain Controller**
- New **Active Directory forest and domain** created and operational
- **Ping test** confirmed network connectivity between client and server
- Windows 11 PC successfully **joined to the Active Directory domain**
 
---
 
## 📸 Screenshots
 
<p align="center">
  <img src="images/lab2/lab2-image-1.png" width="45%" />
  <img src="images/lab2/lab2-image-2.png" width="45%" />
  <img src="images/lab2/lab2-image-3.png" width="45%" />
</p>
 
---
 # 🚀 Lab 3 — User & Organizational Unit (OU) Management
 
## 🖥️ What Was Configured
 
<table>
<tr>
<td width="50%" valign="top">
 
**👤 Created a New User & Logged In**
- Created a new **AD user account** via Active Directory Users and Computers (ADUC)
- Set a secure password and configured login settings
- Logged into the domain-joined Windows 11 PC using the **new user account** to verify access
 
</td>
<td width="50%" valign="top">
 
**🗂️ Set Up an Organizational Unit (OU)**
- Created a dedicated **Organizational Unit** in ADUC to mirror a real-world department structure
- OUs allow administrators to apply **targeted Group Policies** and delegate permissions at a granular level
 
</td>
</tr>
<tr>
<td width="50%" valign="top">
 
**🧑‍💼 Added a Helpdesk User to the OU**
- Created a ** Helpdesk user account ( `John Doe`) ** and placed it inside the newly created OU (`IT_Staff`)
- Organised users by role/department for cleaner AD structure and easier management
 
</td>
<td width="50%" valign="top">
 
**🗝️ Delegated Control**
- Used the **Delegation of Control Wizard** to grant the Helpdesk user specific administrative permissions
- Helpdesk can now perform tasks (e.g., create,edit or delete user,reset passwords) **without needing full Domain Admin rights**
- Follows the **principle of least privilege**
 
</td>
</tr>
</table>
 
---
 
## 📋 Setup Steps
 
**Step 1 — Create a New User Account**
Open **Active Directory Users and Computers (ADUC)** → Expand your domain → Right-click **Users** → **New → User** → Fill in the name and username → Set a password → Finish.
 
**Step 2 — Log In with the New Account**
On the domain-joined Windows 11 PC, sign out → Log in with the new domain user credentials (e.g., `INFOTECH/john`) → Verify successful access.
 
**Step 3 — Create an Organizational Unit (OU)**
In **ADUC** → Right-click your domain → **New → Organizational Unit** → Name it (`IT_Staff`) → Click OK.
 
**Step 4 — Add the Helpdesk User to the OU**
In **ADUC** → Right-click **Users** → **New → User** → Create the Helpdesk user → Once created, drag or **move** the user into the new OU, or create it directly inside the OU from the start.
 
**Step 5 — Delegate Control to the Helpdesk User**
Right-click the **OU** in ADUC → **Delegate Control** → Click **Add** → Search for the Helpdesk user → Select the tasks to delegate (e.g., *Reset user passwords and force password change at next logon*) → Complete the wizard.
 
---
 
## ✅ Outcome
 
- New **AD user account** created and login verified on domain-joined Windows 11 PC
- **Organizational Unit (OU)** created to logically structure users by role
- **Helpdesk user** added to the OU for organised, policy-ready management
- **Control delegated** to Helpdesk user — can perform specific admin tasks without Domain Admin privileges
- AD structure now reflects a **real-world least-privilege model**

 
---
 
## 📸 Screenshots
 
<p align="center">
  <img src="images/lab3/lab3-image-1.png" width="45%" />

</p>
 
---

# 🚀 Lab 4 — Active Directory Groups: Security vs Distribution & Adding Users
 
## 🔍 Key Concept: Security vs Distribution Groups
 
<table>
<tr>
<td width="50%" valign="top">
 
**🔒 Security Group — `IT_Staff`**
- Used to **control access to resources** (folders, printers, shared drives)
- Members: **Paula Doe**, **Ram Doe**
- Permissions can be assigned directly to the group — no need to manage individual users
- Best practice for enforcing **least-privilege access**
 
</td>
<td width="50%" valign="top">
 
**📧 Distribution Group — `All_Staff`**
- Used for **email distribution lists** — cannot be used to assign resource permissions
- Members: **Paula Doe**, **Ram Doe**, **Dave Doe**
- Ideal for org-wide communications without granting system access
- Not a security boundary — use Security Groups for access control
 
</td>
</tr>
</table>
 
---
 
## 🖥️ What Was Configured
 
<table>
<tr>
<td width="50%" valign="top">
 
**👥 Created Groups & Added Users**
- Created `IT_Staff` as a **Security Group** and `All_Staff` as a **Distribution Group** in ADUC
- Added **Paula Doe** and **Ram Doe** to `IT_Staff`
- Added **Paula Doe**, **Ram Doe**, and **Dave Doe** to `All_Staff`
 
</td>
<td width="50%" valign="top">
 
**📁 Created & Shared a Folder**
- Created folder `IT_Docs` on Windows Server 2025 with a file `HelloPaula.txt` inside
- Shared `IT_Docs` with the `IT_Staff` Security Group with **Read access** via folder Properties → Sharing
- Only members of `IT_Staff` can access the shared folder
 
</td>
</tr>
<tr>
<td width="50%" valign="top">
 
**✅ Verified Access — Paula Doe**
- Logged into the Windows 11 client machine using **Paula's** domain account
- Successfully accessed the `IT_Docs` shared folder and the `HelloPaula.txt` file
- Confirmed Security Group permissions are working correctly
 
</td>
<td width="50%" valign="top">
 
**❌ Verified Access Denied — Dave Doe**
- Logged into the Windows 11 client machine using **Dave's** domain account
- Access to `IT_Docs` was **denied** — as expected, since Dave is not a member of `IT_Staff`
- Demonstrates how Security Groups enforce access boundaries effectively
 
</td>
</tr>
</table>
 
---
 
## 📋 Setup Steps
 
**Step 1 — Create the Security Group (`IT_Staff`)**
In **ADUC** → Expand your domain → Right-click **Users** (or your OU) → **New → Group** → Name it `IT_Staff` → Set *Group type* to **Security** → Set *Group scope* to **Global** → Click OK.
 
**Step 2 — Create the Distribution Group (`All_Staff`)**
Repeat the same process → Name it `All_Staff` → Set *Group type* to **Distribution** → Click OK.
 
**Step 3 — Create Users & Add to Groups**
Create users **Paula Doe**, **Ram Doe**, and **Dave Doe** in ADUC. Then:
 
| User | IT_Staff (Security) | All_Staff (Distribution) |
|------|:-------------------:|:------------------------:|
| Paula Doe | ✅ | ✅ |
| Ram Doe | ✅ | ✅ |
| Dave Doe | ❌ | ✅ |
 
To add members: Right-click the group → **Properties → Members tab → Add** → Search for the user → OK.
 
**Step 4 — Create the Shared Folder on the Server**
On Windows Server 2025, create a folder named `IT_Docs` → Inside, create a text file named `HelloPaula.txt` → Right-click `IT_Docs` → **Properties → Sharing tab → Share** → Type `IT_Staff` → Set permission to **Read** → Share.
 
**Step 5 — Verify Access as Paula Doe (IT_Staff Member)**
On the Windows 11 client, log in as **Paula Doe** → Open **File Explorer** → Navigate to `\\VM-DEV-SERV-\IT_Docs` → Confirm the folder and `HelloPaula.txt` are accessible. ✅
 
**Step 6 — Verify Access Denied as Dave Doe (Non-Member)**
Log out → Log in as **Dave Doe** → Attempt to access `\\VM-DEV-SERV-\IT_Docs` → Access should be **denied**, confirming the Security Group restriction is working as intended. ❌
 
---
 
## ✅ Outcome
 
- Understood the difference between **Security Groups** (access control) and **Distribution Groups** (email only)
- Created `IT_Staff` (Security) and `All_Staff` (Distribution) with the correct group types
- Added **Paula** and **Ram** to `IT_Staff`; all three users added to `All_Staff`
- Created shared folder `IT_Docs` with `HelloPaula.txt` and restricted access to `IT_Staff`
- **Paula** successfully accessed the shared folder ✅ — **Dave** was correctly denied ❌
- Demonstrated how **AD Security Groups make access management scalable and secure**
- Ready for **Group Policy Configuration** *(Lab 5)*
 
---
 
## 📸 Screenshots
 
<p align="center">
  <img src="images/lab4/lab4-image-1.png" width="45%" />
  <img src="images/lab4/lab4-image-2.png" width="45%" />
  <img src="images/lab4/lab4-image-3.png" width="45%" />
  <img src="images/lab4/lab4-image-4.png" width="45%" />
  <img src="images/lab4/lab4-image-5.png" width="45%" />
</p>
 
---

