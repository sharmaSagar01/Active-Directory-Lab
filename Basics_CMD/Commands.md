# 🖥️ IT Command Reference — Helpdesk, Windows Server & Active Directory

> A practical command reference for **Helpdesk Technicians**, **Windows Server Admins**, and **Active Directory Support** engineers.

<div align="center">

![Helpdesk](https://img.shields.io/badge/Role-Helpdesk%20Tech-blue?style=flat-square&logo=windows)
![Server Admin](https://img.shields.io/badge/Role-Server%20Admin-darkgreen?style=flat-square&logo=windows-terminal)
![AD Support](https://img.shields.io/badge/Role-AD%20Support-8B0000?style=flat-square&logo=microsoft)

</div>

---

## ⚡ Phase 1 — Network & System Troubleshooting

| Command | Purpose |
|---------|---------|
| `ipconfig /all` | Show full network details of a PC |
| `ping <IP>` | Test network connectivity |
| `sfc /scannow` | Scan & repair Windows system files |
| `netstat -ano` | View all active connections with PIDs |
| `tasklist \| findstr "process.exe"` | Check if a specific process is running |

---

### 🌐 `ipconfig /all` — Full Network Details

Shows every adapter's IP, subnet, gateway, DNS, MAC, and DHCP status in one shot.

```cmd
ipconfig /all
```

**Example output (Client PC):**
```
IPv4 Address  . . . : 192.168.1.105
Subnet Mask . . . . : 255.255.255.0
Default Gateway . . : 192.168.1.1
DNS Servers . . . . : 192.168.1.10
DHCP Enabled  . . . : Yes
```
> 💡 If the IP starts with `169.254.x.x` — DHCP has failed. Run `ipconfig /renew` to request a new lease.

---

### 📡 `ping` — Test Connectivity

Sends ICMP packets to verify a host is reachable. Tested against `8.8.8.8` (Google DNS) to confirm internet access.

```cmd
ping 8.8.8.8          # Test internet connectivity
ping 192.168.1.10     # Test connectivity to a server
ping -t 192.168.1.10  # Continuous ping (Ctrl+C to stop)
```

> 💡 `Request timed out` = host unreachable or blocked by firewall. `could not find host` = DNS failure.

---

### 🛠️ `sfc /scannow` — Repair System Files

Scans all protected Windows system files and repairs corruption automatically. Run as **Administrator**.

```cmd
sfc /scannow
```

**Result from this lab:** No integrity violations found — system files are healthy ✅

> 💡 If SFC can't repair files, run `DISM /Online /Cleanup-Image /RestoreHealth` first, then retry.

---

### 🔌 `netstat -ano` — Active Connections

Lists all active TCP/UDP connections, listening ports, and the Process ID (PID) owning each.

```cmd
netstat -ano                          # All connections
netstat -ano | findstr "LISTENING"    # Only listening ports
netstat -ano | findstr ":3389"        # Check if RDP is active
```

> 💡 Note the PID from the output, then use `tasklist | findstr "<PID>"` to identify which process owns that port.

---

### 🔍 `tasklist | findstr` — Find a Running Process

Filters the full process list to check if a specific application is running.

```cmd
tasklist | findstr "msedge.exe"    # Check if Microsoft Edge is running
tasklist | findstr "Teams.exe"     # Check if Teams is running
```

> 💡 To force-kill a stuck process: `taskkill /IM "msedge.exe" /F`

---

## ⚙️ Phase 2 — Windows Server 2025 Essential Admin Commands

| Command | Purpose |
|---------|---------|
| `systeminfo` | Full server snapshot — OS, uptime, hotfixes, hardware |
| `sconfig` | Configure hostname, domain, updates & RDP from CMD |
| `ipconfig /all` | Full network details for DNS, DHCP & connectivity |
| `net user /domain` | View or manage domain/local user accounts |
| `net group /domain` | Check AD group memberships |
| `gpupdate /force` | Apply Group Policy instantly |
| `quser` | See who's logged in via RDP |
| `cleanmgr /sagerun` | Automate disk cleanup on the server |
| `dism /online /cleanup-image /restorehealth` | Repair corrupted Windows system files |
| `net statistics server` | Check uptime, sessions & server performance |

---

### 🧩 `systeminfo` — Server Snapshot

Full overview of the server — OS version, uptime, installed hotfixes, RAM, and hardware details.

```cmd
systeminfo
systeminfo | findstr "OS"         # Filter to OS info only
systeminfo | findstr "Hotfix"     # List installed patches
```

> 💡 Great first command when inheriting a server you've never managed before.

---

### ⚙️ `sconfig` — Server Configuration Menu

Interactive menu for common server setup tasks without opening the GUI.

```cmd
sconfig
```

Covers: hostname, domain join, Windows Updates, Remote Desktop, and network settings — all from CMD.

> 💡 Essential on **Server Core** installs where there is no Desktop Experience GUI.

---

### 👤 `net user` — User Account Management

View and manage local or domain user accounts directly from CMD.

```cmd
net user                          # List all local users
net user sue /domain              # View details of domain user Sue
net user sue NewPass123 /domain   # Reset Sue's password
```

> 💡 Faster than opening ADUC for a quick password reset or account check.

---

### 🛡️ `net group` — AD Group Management

Check group memberships and manage AD groups without opening ADUC.

```cmd
net group /domain                        # List all domain groups
net group "IT_Staff" /domain             # See members of IT_Staff group
net group "IT_Staff" sue /add /domain    # Add Sue to IT_Staff
```

> 💡 Useful when troubleshooting access issues — quickly verify if a user is in the right group.

---

### 🔒 `gpupdate /force` — Apply Group Policy Instantly

Forces an immediate Group Policy refresh — no waiting for the default 90-minute cycle.

```cmd
gpupdate /force
gpupdate /force /logoff    # Force update then log off (required for some User policies)
```

> 💡 Always run this after making GPO changes and before testing — on both server and client.

---

### 🖥️ `quser` — Who's Logged In via RDP

Shows all active and disconnected RDP sessions on the server, including session ID and idle time.

```cmd
quser
logoff 2    # Log off session ID 2 (replace with actual ID from quser output)
```

> 💡 Use `logoff <ID>` to cleanly end a disconnected or stuck RDP session.

---

### 🧹 `cleanmgr /sagerun` — Automated Disk Cleanup

Runs disk cleanup with a pre-configured profile — removes temp files, old logs, and Windows update cache.

```cmd
cleanmgr /sageset:1     # Configure what to clean (run once to set up)
cleanmgr /sagerun:1     # Run cleanup automatically using saved settings
```

> 💡 Schedule `sagerun` as a Task Scheduler job for hands-free regular cleanup on busy servers.

---

### 🩺 `dism /online /cleanup-image /restorehealth` — Repair Windows Image

Repairs the Windows component store — use when `sfc /scannow` finds corruption it can't fix.

```cmd
dism /online /cleanup-image /restorehealth
sfc /scannow    # Re-run SFC after DISM completes
```

> 💡 DISM pulls repair files from **Windows Update** — the server needs internet access to run this.

---

### 📊 `net statistics server` — Server Uptime & Performance

Quick snapshot of server activity — sessions, bytes transferred, errors, and how long it's been running.

```cmd
net statistics server
net statistics server | findstr "since"    # Show uptime start time only
```

> 💡 Use this before a maintenance window to confirm no active sessions are open.

---

## 🗂️ Phase 3 — Active Directory CMD Commands

| Command | Purpose |
|---------|---------|
| `dsquery user` | Find any user account in the domain |
| `dsquery computer` | Find any computer account in the domain |
| `net user /domain` | Quick domain user lookup & audit |
| `net group /domain` | Check AD group memberships from CMD |
| `nltest /dsgetdc:infotech.com` | Locate an available Domain Controller |
| `gpresult /r` | See which GPOs are applied to the current user/machine |

---

### 🟦 `dsquery user` — Find User Accounts in AD

Search for any user account across the domain without opening ADUC.

```cmd
dsquery user                              # List all users in the domain
dsquery user -name "sue*"                 # Find users whose name starts with Sue
dsquery user -inactive 4                  # Find accounts inactive for 4+ weeks
dsquery user -disabled                    # Find all disabled accounts
```

> 💡 Chain with `dsget` to pull more details: `dsquery user -name "sue*" | dsget user -email -tel`

---

### 🟦 `dsquery computer` — Find Computer Accounts in AD

Quickly locate machine accounts — useful when troubleshooting domain-join issues.

```cmd
dsquery computer                          # List all computers in the domain
dsquery computer -name "CLIENT*"          # Find computers starting with CLIENT
dsquery computer -inactive 4              # Find machines inactive for 4+ weeks
```

> 💡 A missing computer account here means the machine isn't domain-joined or was removed from AD.

---

### 🟨 `net user` & `net group` — Quick AD Audit

Already covered in Phase 2 — equally critical for AD support. Quick reference:

```cmd
net user sue /domain              # View Sue's account details, expiry, last logon
net group "IT_Staff" /domain      # List all members of the IT_Staff group
net group /domain                 # List every group in the domain
```

> 💡 Fastest way to confirm if a user is in the right group when troubleshooting access issues.

---

### 🟧 `nltest /dsgetdc` — Locate a Domain Controller

Returns the name and IP of an available DC for a given domain. Essential for diagnosing login delays and replication issues.

```cmd
nltest /dsgetdc:infotech.com              # Find any available DC
nltest /dsgetdc:infotech.com /force       # Bypass cache and rediscover
```

**Example output:**
```
DC: \\VM-DEV-WINSERV-01
Address: \\192.168.1.10
Dom Guid: ...
The command completed successfully
```

> 💡 If this fails or returns a slow DC, it often explains why logins and GPOs are taking unusually long.

---

### 🟩 `gpresult /r` — Check Applied GPOs

Shows exactly which Group Policies are applied to the current user and machine — no GPMC needed.

```cmd
gpresult /r                        # Applied GPOs for current user & machine
gpresult /r /user:sue              # Check GPOs applied to a specific user
gpresult /h report.html            # Export a full HTML GPO report
```

> 💡 If a GPO isn't showing up here after `gpupdate /force`, it's either not linked, filtered, or scoped to the wrong OU.

---

