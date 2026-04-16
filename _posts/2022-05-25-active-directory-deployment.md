---
title: "Microsoft Active Directory: Deploying Centralized Authentication for an Enterprise Network"
date: 2022-05-25 00:00:00 +0000
categories: [Projects]
tags: [active-directory, windows-server, ad-ds, dhcp, dns, gpo, wsus, vpn, radius, ldap, vmware, networking]
image:
  path: /assets/images/active-directory/ad-visio-2.jpeg
  alt: Active Directory network diagram
---

This project was done during my first year in the Networks, Systems & Security program at IPNET Institute. The goal was to deploy a full Microsoft Active Directory infrastructure from scratch, document it end to end, and demonstrate its working capabilities to the supervising engineer.

Team: **ASSIMTI Charles** (group lead), LANTSIGBLE Kodjo, ANAKPA Wiyao.
Supervisor: **Mr. Jonas YANKIKA**, Senior Engineer, IPNET Institute.

---

## What is Active Directory and why it matters

Active Directory (AD) is a directory service that runs on Windows Server. It gives a company one place to manage all its users, computers, and applications. Instead of each machine having its own local accounts and policies, everything is centralized: one user account, one password, one set of rules applied across the whole network.

The core benefits:

- **Centralized authentication**: every user authenticates against the AD server, whether they're on a wired workstation, WiFi, or connecting remotely via VPN
- **Group policies (GPOs)**: push settings and restrictions to machines and users without touching each one individually
- **Resource access control**: control exactly who can access which folders, printers, or applications
- **Automated updates (WSUS)**: manage Windows Update centrally instead of letting each machine pull from Microsoft

The scenario for this project: a fictional institution called **LPRC** needs to implement this solution. We were hired as integrators at IPNET and tasked with deploying it on IPNET's own infrastructure first, then handing over a full deployment package.

---

## Network diagram

<img src="/assets/images/active-directory/ad-visio-2.jpeg" alt="Active Directory network architecture diagram" style="border-radius: 10px; width: 80%;" />

The architecture centers on a **Windows Server 2016** domain controller running AD DS, DNS, DHCP, WSUS, and Remote Access. Client machines (Windows 10) join the domain and receive their configuration via DHCP and GPO. A Cisco Catalyst switch handles LAN connectivity, Cisco access points cover WiFi, and a Cisco Firepower handles the perimeter. The VPN gateway allows remote users to authenticate against the AD server over IPSec.

Protocols in use: **DNS, DHCP, LDAP, Kerberos, RADIUS, TCP/IP**.

---

## Bill of Materials (BOM)

The project included a full commercial offer with pro-forma invoice. Here is the equipment list:

| Reference | Description | Qty |
|-----------|-------------|-----|
| Cisco Catalyst C9200L-24P-4G-E | 24-port PoE+ switch, Network Advantage | 1 |
| HPE ProLiant ML30 Gen10 (P16929-421) | Server, E-2234, 16GB RAM, 4-bay LFF | 1 |
| Windows Server 2016 Datacenter (P71-08671) | 24 cores | 1 |
| Cisco Firepower 1010 (FPR1010-NGFW-K9) | NGFW appliance, 8x GbE, up to 650 Mbps | 1 |
| Cisco C9115AX-EWC Access Points | Embedded wireless controller | 9 |
| Cat6 FTP cable (touret 1000m) | Backbone cabling | 1 |
| Cat6 patch panel 24 ports | Rack patching | 1 |
| Legrand RJ45 Cat6 outlets | Wall outlets | 30 |
| Coffret 6U 19" | Network rack | 1 |
| Kaspersky Security Center 13 | Antivirus management | 1 |
| All-in-one PC (Intel i7, 23.8", Win10) | Client workstations | 10 |
| Windows 10 Enterprise + Pro 64-bit | Client OS | 4+4 |

**Total (pro-forma invoice):** 39,108,394 FCFA TTC (18% VAT included).
Payment terms: 75% on order, 25% on delivery. Lead time: 6 weeks.

---

## Deployment

### Virtual environment setup

For the lab deployment, we used **VMware Workstation Pro** to run the server and client machines locally. Two VMs were created: one for Windows Server 2016 (named `SRV_PPE_AD`) and one for Windows 10 (the client). Both were connected to a custom VMnet (`VMnet2`, network `10.0.254.0/24`, NAT mode) so they could communicate.

The server was given a static IP: `10.0.254.17`, with itself as the DNS server. The client was configured to use this same address as its DNS.

<img src="/assets/images/active-directory/ad-manuel-3.png" alt="VMware virtual network configuration" style="border-radius: 10px; width: 80%;" />

<img src="/assets/images/active-directory/ad-manuel-5.png" alt="Windows Server VM setup" style="border-radius: 10px; width: 80%;" />

---

### AD DS and DNS installation

From Server Manager, we added the **AD DS** role. DNS installs automatically alongside it — AD relies on DNS for domain resolution, so they go together.

<img src="/assets/images/active-directory/ad-manuel-10.png" alt="Adding the AD DS role" style="border-radius: 10px; width: 80%;" />

After installation, the server was promoted to **domain controller** and a new forest was created. Domain name: `ppe.local`. After the promotion completes the machine reboots, and from that point on the administrator session shows the domain name.

<img src="/assets/images/active-directory/ad-manuel-15.png" alt="Promoting server to domain controller" style="border-radius: 10px; width: 80%;" />

<img src="/assets/images/active-directory/ad-manuel-17.png" alt="Domain controller promotion completed" style="border-radius: 10px; width: 80%;" />

---

### DHCP

DHCP was added as a separate role. After installation, a new scope was created: **DHCP_POOL_AD**, range `10.0.254.0/24`, with the server itself (`10.0.254.17`) as gateway and DNS server. Clients on the domain now get addresses automatically.

<img src="/assets/images/active-directory/ad-manuel-22.png" alt="DHCP scope configuration" style="border-radius: 10px; width: 80%;" />

---

### Users, Groups, and Organizational Units

Under **Active Directory Users and Computers**, we created an Organizational Unit (OU) called `SERVICE COMMERCIAL`. Inside it:

- Two users: **Jean ANALA** (comptable) and a second user (secretaire)
- A group: **Groupe Comptabilité**, with both users as members

OUs are the building block of AD. They let you apply GPOs to specific sets of users or machines rather than the whole domain.

<img src="/assets/images/active-directory/ad-manuel-43.png" alt="Creating organizational unit SERVICE COMMERCIAL" style="border-radius: 10px; width: 80%;" />

<img src="/assets/images/active-directory/ad-manuel-47.png" alt="User creation in AD" style="border-radius: 10px; width: 80%;" />

<img src="/assets/images/active-directory/ad-manuel-55.png" alt="Group Comptabilité created with members" style="border-radius: 10px; width: 80%;" />

---

### Joining the client to the domain

On the Windows 10 machine, we changed the DNS to point at `10.0.254.17`, then joined the domain `ppe.local`. This requires domain admin credentials. After a reboot, the login screen shows an "Other user" option — logging in with a domain account (`comptable`) works, and the machine properties confirm it belongs to `ppe.local`.

<img src="/assets/images/active-directory/ad-manuel-67.png" alt="Client machine joined to domain ppe.local" style="border-radius: 10px; width: 80%;" />

---

### Group Policy Objects (GPOs)

GPOs are how you push settings to users and machines without touching each one. We deployed four:

**1. Custom wallpaper (`GPO_U_fondEcran`)**

A company wallpaper was placed in a shared folder on the server (`DEPLOY`), then a GPO configured to push it to the `Groupe Comptabilité`. After the client runs `gpupdate /force`, the wallpaper changes automatically.

<img src="/assets/images/active-directory/ad-manuel-73.png" alt="GPO wallpaper configuration" style="border-radius: 10px; width: 80%;" />

<img src="/assets/images/active-directory/ad-manuel-80.png" alt="Wallpaper GPO applied on client machine" style="border-radius: 10px; width: 80%;" />

**2. Block CMD access (`GPO_U_CMD`)**

A GPO was linked to the `SERVICE COMMERCIAL` OU disabling access to the command prompt for all users in that unit. Testing confirmed: trying to open CMD on the client machine results in an error.

<img src="/assets/images/active-directory/ad-manuel-82.png" alt="CMD blocked via GPO on client" style="border-radius: 10px; width: 80%;" />

**3. Block USB storage (`GPO_U_USB`)**

A GPO was applied to block all removable storage classes. Plugging in a USB drive on the client machine shows an access denied error. This is a common enterprise policy to prevent data exfiltration.

<img src="/assets/images/active-directory/ad-manuel-87.png" alt="USB access denied via GPO" style="border-radius: 10px; width: 80%;" />

**4. Deploy Firefox ESR by GPO**

An MSI installer for Firefox ESR was placed in a shared `Applications` folder on the server. A GPO configured under **Computer Configuration > Software Installation** pushed the package to all domain computers. Running `gpupdate /force` on the client triggers the installation on next login — no manual install needed on each machine.

<img src="/assets/images/active-directory/ad-manuel-91.png" alt="Firefox ESR GPO software deployment" style="border-radius: 10px; width: 80%;" />

<img src="/assets/images/active-directory/ad-manuel-92.png" alt="Firefox ESR installed on client after GPO" style="border-radius: 10px; width: 80%;" />

---

### WSUS: centralized Windows Update management

WSUS (Windows Server Update Services) lets you control which updates get pushed to which machines, and when. We installed the WSUS role on the server, configured it to sync from Microsoft Update, and created two groups: **Computers** and **Servers**.

Three GPOs were created to manage updates:
- `GPO Computers`: targets workstations, blocks auto-restart during active hours, sets client-side targeting to "Computers" group
- `GPO Servers`: same logic for servers
- `GPO Communs`: shared policy for both, sets update schedule and points all machines to the internal WSUS server instead of Microsoft's servers directly

<img src="/assets/images/active-directory/ad-manuel-111.png" alt="WSUS console showing update groups" style="border-radius: 10px; width: 80%;" />

<img src="/assets/images/active-directory/ad-manuel-113.png" alt="WSUS GPO configuration" style="border-radius: 10px; width: 80%;" />

On the client machine, Windows Update settings confirm the policy: updates are managed by the organization, not pulled independently from Microsoft.

<img src="/assets/images/active-directory/ad-manuel-126.png" alt="Windows Update managed by organization on client" style="border-radius: 10px; width: 80%;" />

---

### VPN remote access

The **Remote Access** role (DirectAccess + VPN) was installed and configured with the **Routing and Remote Access** wizard. A Network Policy (NPS) was created to allow only the `Groupe Comptabilité` to connect via VPN.

On the Windows 10 client, we added a VPN connection (L2TP/IPSec) pointing at the server's IP. Connecting with the `comptable` account succeeds. Trying with the `secretaire` account (not in the allowed group) fails — which is exactly what the policy should do.

<img src="/assets/images/active-directory/ad-manuel-147.png" alt="VPN connection configured on Windows 10 client" style="border-radius: 10px; width: 80%;" />

<img src="/assets/images/active-directory/ad-manuel-159.png" alt="VPN connection established with domain account" style="border-radius: 10px; width: 80%;" />

---

## Test validation (cahier de recettes)

The deployment was validated against 10 defined tests:

| # | Test | Result |
|---|------|--------|
| 1 | Create user account with expiry date | Passed |
| 2 | WiFi authentication via RADIUS | Passed |
| 3 | Configure RADIUS client and 802.1x/Ethernet | Passed |
| 4 | User authentication via LDAP | Passed |
| 5 | Centralized updates via WSUS | Passed |
| 6 | Block USB storage access | Passed |
| 7 | Block user access to specific network resources | Passed |
| 8 | Connect user via IPSec VPN | Passed |
| 9 | Centralized application installation (Firefox ESR) | Passed |
| 10 | Deploy custom wallpaper via GPO | Passed |

---

## Project timeline

The project ran across two academic years following a structured progression:

| Phase | Progress | Deadline | Status |
|-------|----------|----------|--------|
| Protocol mastery (DNS, DHCP, LDAP, Kerberos, RADIUS) | +10% | 13/05/2022 | Done |
| Architecture understanding and whiteboard explanation | +20% | 26/06/2022 | Done |
| Lab deployment across 6 configuration stages | +20% | 03/11/2022 | Done |
| Deployment manual + commercial offer + cahier de recettes | +25% | 09/01/2023 | Done |
| Full production deployment on IPNET network | +25% | — | Done |

Each phase was signed off by Mr. Jonas YANKIKA before moving to the next.

---

## What I took away from this

Active Directory is one of those technologies that shows up everywhere — in almost every enterprise network — and yet you don't really understand it until you've deployed it yourself, broken it a few times, and debugged why the GPO isn't applying or why DNS resolution fails. 

This project forced us to touch every layer: the server roles, the network configuration, the user management, the policy engine, and the update infrastructure. Writing the full deployment manual and the commercial offer on top of that gave a perspective that goes beyond just the technical side — it made me think about how this kind of solution gets scoped, priced, and handed over to a client.
