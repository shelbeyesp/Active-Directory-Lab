# Active-Directory-Lab
Independent project: Active Directory home lab
# Windows Active Directory Home Lab (`lab.local`)

A hands-on Active Directory environment built in VirtualBox to practice core Windows Server administration, static network configuration, Domain Controller promotion, and identity and access management.

## Lab Architecture & Specs

* **Hypervisor:** Oracle VM VirtualBox
* **Domain Name:** `lab.local`
* **Network Subnet:** `192.168.10.0/24` (Internal Network)

| Hostname | Role | Operating System | IP Address | Specs |
| :--- | :--- | :--- | :--- | :--- |
| `DC01` | Primary Domain Controller & DNS | Windows Server 2022 Standard | `192.168.10.10` | ~3.5 GB RAM, 60 GB Disk |
| `CLIENT01` | Domain Client Workstation | Windows 11 | `192.168.10.x` | VirtualBox Guest |

---

## Build & Configuration Walkthrough

### 1. Static IP & Network Configuration
Configured a static IPv4 address (`192.168.10.10/24`) on the server's Ethernet adapter to ensure consistent network addressing and DNS resolution for domain clients.

![Static IP Configuration](docs/screenshots/01-static-ip.png)
*Figure 1: Configuring static IPv4 settings (192.168.10.10 / 255.255.255.0) on the server interface.*

---

### 2. Server Renaming & Preparation
Renamed the host machine from its default random Windows hostname to `DC01` and verified system properties in Server Manager prior to installing Active Directory Domain Services (AD DS).

![Renamed Server DC01](docs/screenshots/02-renamed-server.png)
*Figure 2: Server Manager Local Server dashboard confirming the hostname update to `DC01` and static IP assignment.*

---

### 3. Active Directory Domain Services (AD DS) & DNS Verification
Installed the AD DS and DNS server roles, promoted `DC01` to a Domain Controller in a new forest (`lab.local`), and verified the default domain containers inside Active Directory Users and Computers (ADUC).

![Verify AD and DNS](docs/screenshots/03-verify-ad-dns.png)
*Figure 3: Active Directory Users and Computers confirming successful deployment of the `lab.local` domain.*

---

### 4. Client-to-Domain Controller Connectivity Testing
Verified network reachability from the Windows 11 client workstation to `DC01` (`192.168.10.10`) via ICMP ping, confirming 0% packet loss across the internal VirtualBox network.

![Testing Connectivity](docs/screenshots/04-testing-connectivity.png)
*Figure 4: Successful ICMP ping test from the Windows 11 client to DC01 (192.168.10.10) with 0% packet loss.*

---

### 5. User Accounts & Security Group Provisioning
Created domain user accounts (`Jane Doe`, `Mike James`) and role-based security groups (`IT Admins`, `Staff`) in `lab.local` to establish Role-Based Access Control (RBAC).

![AD Users and Security Groups](docs/screenshots/05-users-and-groups.png)
*Figure 5: Active Directory Users and Computers showing provisioned user accounts and `IT Admins` / `Staff` security groups.*

### 6. Client Domain Join & User Authentication Verification
Joined the Windows 11 client workstation to `lab.local` and verified domain authentication by logging in with the newly provisioned user accounts (`Jane Doe` and `Mike James`) and confirming domain identity via `whoami`

---

## Troubleshooting & Lessons Learned

* **DNS IP Typo Caught During Verification:** While configuring static IPv4 properties on `DC01`, a typo (`192.162.10.10` instead of `192.168.10.10`) was initially entered in the Preferred DNS Server field. Reviewing network settings before promotion ensured DNS pointed cleanly to `192.168.10.10`.
* **Internal Network Isolation:** Configured both VMs on an isolated VirtualBox Internal Network so DHCP/DNS traffic from the host network would not interfere with `lab.local` name resolution.
