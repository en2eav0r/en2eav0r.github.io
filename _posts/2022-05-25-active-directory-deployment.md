---
title: "Microsoft Active Directory: deploying centralized authentication for an enterprise network"
date: 2022-05-25 00:00:00 +0000
categories: [Projects]
tags: [active-directory, windows-server, ad-ds, dhcp, dns, gpo, wsus, vpn, radius, ldap, vmware, networking]
image:
  path: /assets/images/active-directory/ad-visio-2.jpeg
  alt: Active Directory network diagram
---

First-year project at IPNET Institute. The goal was to deploy a full Microsoft Active Directory infrastructure from scratch on a virtualized lab, document it end to end, write the commercial offer, and present a working demo to the supervising engineer.

Team: **ASSIMTI Charles** (group lead), LANTSIGBLE Kodjo, ANAKPA Wiyao.
Supervisor: **Mr. Jonas YANKIKA**, Senior Engineer, IPNET Institute.
Client scenario: a fictional institution called **LPRC**, contracted through IPNET.

---

## What is Active Directory and why it matters

Active Directory is a directory service that runs on Windows Server. Its job is to give an organization one place to manage all users, computers, and resources on the network. Without it, each machine has its own local accounts, its own policies, its own update schedule. With it, you configure things once and they propagate everywhere.

The authentication backbone is **Kerberos**. When a domain user logs in, their workstation contacts the domain controller, which issues a Ticket Granting Ticket (TGT). That ticket gets presented to any service on the network to prove identity, without re-entering a password each time. LDAP handles directory lookups, DNS handles name resolution between services, and RADIUS handles authentication for WiFi and VPN connections.

For LPRC, the payoff is concrete: one user account per person, one password, working from any workstation in the building or over VPN from home. Group Policy pushes configurations automatically across the fleet. Access to shared folders is controlled by group membership, not per-machine permissions. Windows updates go through WSUS so the IT team decides what ships and when, rather than leaving each machine to do its own thing.

This is standard in enterprise environments, but understanding it from a diagram is different from having built it and broken it a few times.

---

## Network diagram

The architecture has a **Windows Server 2016** domain controller at the center, running AD DS, DNS, DHCP, WSUS, and Remote Access all on one machine. For a production deployment of this scale, you would typically separate these roles. For this project, colocating them was intentional, it demonstrated how each role interacts with the others and made the dependencies visible.

Client workstations (Windows 10) join the domain and get their IP address and DNS settings from the DHCP server. All name resolution goes through the domain controller's DNS, which is why every client must point at it as the primary DNS. If DNS fails, the domain breaks, authentication fails, and GPOs cannot apply.

Network infrastructure: Cisco Catalyst switch for LAN, Cisco access points for WiFi with 802.1x authentication via RADIUS, Cisco Firepower at the perimeter. Remote access goes over L2TP/IPSec VPN, with the NPS role on the domain controller handling RADIUS authentication for both WiFi and VPN.

---

## Bill of Materials

The project included a full commercial offer with a pro-forma invoice, priced for the LPRC deployment scenario. Here is the equipment list:

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

**Total (pro-forma invoice): 39,108,394 FCFA TTC** (18% VAT included).
Payment terms: 75% on order, 25% on delivery. Lead time: 6 weeks.

The HPE ProLiant ML30 was chosen because it is a mid-range tower server, affordable for institutions of this size, and already certified for Windows Server 2016. The Cisco C9200L handles both PoE (to power the access points) and a 4-port SFP+ uplink for future fiber expansion. Nine access points for a building that size is not excessive: 802.1x means every WiFi connection authenticates individually against the domain controller via RADIUS, so coverage and capacity matter.

---

## Deployment

### Virtual environment

The lab ran on **VMware Workstation Pro**. Two virtual machines: one for Windows Server 2016 (hostname `SRV_PPE_AD`) and one for Windows 10 (the domain client). Both were connected to a custom virtual network, `VMnet2`, set to NAT mode so they could reach each other and the internet without conflicting with the host machine's physical network.

Network range: `10.0.254.0/24`. The server got a static address: `10.0.254.17`. It was configured to use itself as its own DNS server, which is correct behavior for a domain controller. The Windows 10 VM was set to obtain an IP from DHCP but use `10.0.254.17` as the DNS server, which is what allows it to find and join the domain later.

NAT mode on VMnet2 means both VMs share the host's internet connection through a virtual gateway. Fine for a lab. In production the domain controller goes on a properly routed segment, not behind NAT.

---

### AD DS and DNS installation

From Server Manager, we added the **Active Directory Domain Services** role. DNS installs alongside it automatically. AD relies on DNS for everything: domain controllers register SRV records so clients can find them, Kerberos uses DNS to locate the Key Distribution Center, LDAP queries resolve via DNS. If you separate the two, you need to configure conditional forwarders and make sure records stay in sync. Running DNS on the domain controller keeps all of that automatic.

After the role installed, the server was promoted to domain controller using the **Active Directory Domain Services Configuration Wizard**. A new forest was created with the root domain `ppe.local`. The choice of `.local` for a lab environment is common, but in production it is worth noting that `.local` can conflict with mDNS (Bonjour, Avahi), so Microsoft now recommends using a subdomain of a real registered domain like `corp.example.com`.

After promotion, the machine rebooted. From that point the administrator session shows the domain name in the login screen, and the DNS zone `ppe.local` appears in the DNS Manager with auto-registered records for the domain controller.

---

### DHCP

DHCP was added as a separate role. After installation, a new scope was created: **DHCP_POOL_AD**, covering the range `10.0.254.0/24`. The scope options set the server's own IP (`10.0.254.17`) as both the default gateway and the DNS server for clients.

In a production deployment you would typically exclude the static addresses already in use from the DHCP range (the server itself, routers, printers) to avoid address conflicts. Here the server's static IP `10.0.254.17` was outside the dynamic allocation range. Clients on the domain now get addresses and DNS configuration automatically without any manual setup on each workstation.

---

### Users, groups, and organizational units

Organizational Units are the containers that make Group Policy meaningful. Without OUs, you can only apply policies at the domain level, which means every machine and every user gets the same settings. OUs let you slice the directory into logical groups and apply different policies to each.

We created an OU called `SERVICE COMMERCIAL` at the domain root. Inside it:

- User `Jean ANALA` (comptable)
- User `Marie SOME` (secretaire)
- Group `Groupe Comptabilité`, with both users as members

The group matters because GPOs and access controls should target groups, not individual accounts. When a new accountant joins, you add them to `Groupe Comptabilité` and they inherit every permission and policy that group carries, without touching any of the GPOs or ACLs themselves.

---

### Joining the client to the domain

On the Windows 10 machine, the DNS was pointed at `10.0.254.17` first. This is a step people skip and then spend an hour debugging. If the client cannot resolve `ppe.local` via DNS, the domain join fails with a misleading error.

With DNS confirmed working, we joined the domain through System Properties. The wizard prompts for domain admin credentials. A reboot follows. After the reboot, the Windows login screen shows an "Other user" option alongside the local accounts, and logging in with `ppe.local\comptable` works. The System Properties page confirms the machine belongs to `ppe.local`.

Under the hood what happened: the workstation's Netlogon service contacted a domain controller via DNS, authenticated with the provided credentials, and the DC created a computer account for the workstation in Active Directory. From that point the machine is domain-joined and subject to all GPOs targeted at it.

---

### Group Policy Objects

Four GPOs were deployed across the lab, covering policies you would actually find in enterprise environments.

**Wallpaper via GPO**

A shared folder named `DEPLOY` was created on the server and a wallpaper image placed inside it with read permissions for domain users. A GPO named `GPO_U_fondEcran` was configured under User Configuration, linked to `SERVICE COMMERCIAL`. The wallpaper path pointed to the UNC share (`\\SRV_PPE_AD\DEPLOY\wallpaper.jpg`). Running `gpupdate /force` on the client machine changed the wallpaper without any local configuration.

**Block command prompt**

`GPO_U_CMD` targeted the `SERVICE COMMERCIAL` OU and disabled access to the command prompt for all users in that unit. The setting is under User Configuration > Policies > Administrative Templates > System > Prevent access to the command prompt. Trying to open `cmd.exe` on the client returns an error message stating the command prompt has been disabled by your administrator. This is a standard hardening measure for non-technical staff who have no business reason to run commands.

**Block USB storage**

`GPO_U_USB` blocked all removable storage device classes. The setting is under Computer Configuration > Policies > Administrative Templates > System > Removable Storage Access. Plugging in a USB drive on the domain-joined machine triggers an "Access Denied" error. This is one of the most common data loss prevention policies in enterprise environments, especially in regulated industries.

**Deploy Firefox ESR by GPO**

An MSI installer for Firefox ESR was placed in a shared `Applications` folder on the server. A software installation policy was configured under Computer Configuration > Policies > Software Settings > Software Installation. The package was added with the UNC path to the MSI file. Running `gpupdate /force` on the client machine followed by a reboot triggered the installation automatically. No local admin interaction needed on the workstation.

Software deployment via GPO works through the Windows Installer service. The domain controller holds the package and the policy tells each machine to fetch and install it at next startup. This is older than modern MDM solutions like Intune, but it works reliably in AD-only environments and does not require internet connectivity.

---

### WSUS: centralized update management

WSUS lets you approve, schedule, and track Windows updates across the entire domain from one console instead of letting each machine pull whatever Microsoft releases on Patch Tuesday.

After installing the WSUS role, we ran the initial synchronization to pull the update catalog from Microsoft. Two computer groups were created: `Computers` (workstations) and `Servers`. Three GPOs handled the client configuration:

`GPO Communs` applied to all domain members: points Windows Update at the internal WSUS server instead of Microsoft's update servers, sets the update detection frequency, and enables client-side targeting so each machine reports itself to the correct WSUS group.

`GPO Computers` refined the policy for workstations: blocks automatic restarts during business hours (8am-6pm), sets active hours so updates do not interrupt work sessions.

`GPO Servers` applied similar logic to the server group with different maintenance windows.

On the client machine, the Windows Update settings panel confirms the policy is in effect: updates are "managed by your organization" and no manual check is possible. In the WSUS console on the server, the workstation appears under the `Computers` group after the next detection cycle.

---

### VPN remote access

The **Remote Access** role was installed with the DirectAccess and VPN option. Routing and Remote Access was then configured with a VPN-only setup, binding to the server's network interface.

NPS (Network Policy Server) was configured to authorize only members of `Groupe Comptabilité` for VPN connections. The policy checks group membership before allowing the connection, so credentials alone are not enough.

On the Windows 10 client, we added a VPN connection manually: L2TP/IPSec, server address `10.0.254.17`, using a pre-shared key. Connecting with `comptable` worked. Connecting with `secretaire` (same domain, different group) was denied at the NPS level with an "access denied" response. That is the policy working correctly: authentication succeeded, but authorization failed because the account is not in the allowed group.

L2TP/IPSec was the right choice here for a Windows-native lab. In production you would often see SSTP (SSL-based, port 443, easier through firewalls) or more recently Always On VPN for modern Windows 10/11 environments.

---

## Test validation

The deployment was validated against 10 acceptance tests defined in the cahier de recettes, signed off by the supervisor:

| # | Test | Result |
|---|------|--------|
| 1 | Create user account with expiry date | Passed |
| 2 | WiFi authentication via RADIUS (802.1x) | Passed |
| 3 | Configure RADIUS client and Ethernet 802.1x | Passed |
| 4 | User authentication via LDAP | Passed |
| 5 | Centralized update management via WSUS | Passed |
| 6 | Block USB storage access via GPO | Passed |
| 7 | Block user access to specific network resources | Passed |
| 8 | Remote access via IPSec VPN | Passed |
| 9 | Centralized application deployment (Firefox ESR) | Passed |
| 10 | Custom wallpaper pushed via GPO | Passed |

Test 2 and 3 (RADIUS/802.1x) were the most involved. They required configuring the Cisco access point as a RADIUS client, pointing it at the NPS server on the domain controller, and setting up a network policy that accepted WiFi authentication requests. Getting the shared secret right between the AP and NPS, and configuring the correct EAP method, took several iterations.

Test 4 (LDAP authentication) verified that an external tool could query the directory using LDAP credentials, which matters for any application that needs to authenticate users against AD without using Kerberos natively.

---

## Project timeline

The project ran over two academic semesters with five structured phases, each signed off before moving to the next:

| Phase | Content | Deadline | Status |
|-------|---------|----------|--------|
| Protocol study | DNS, DHCP, LDAP, Kerberos, RADIUS theory | 13/05/2022 | Done |
| Architecture design | Network topology, whiteboard explanation to supervisor | 26/06/2022 | Done |
| Lab deployment | 6-stage configuration across VMware environment | 03/11/2022 | Done |
| Documentation | Deployment manual, commercial offer, cahier de recettes | 09/01/2023 | Done |
| Production handover | Full deployment on IPNET's live network | | Done |

The documentation phase was underestimated at the start. Writing a proper deployment manual means going back through every step with enough detail that someone who was not present can reproduce it exactly. The commercial offer required sourcing actual part numbers, current pricing from distributors, and calculating margins. The cahier de recettes meant defining what "passed" actually means for each test, not just running it and saying it worked.

---

## What I took away from this

Deploying Active Directory from zero is genuinely one of the better ways to understand how enterprise authentication works. Reading about Kerberos is one thing. Watching the ticket exchange happen in Wireshark while debugging a domain join failure is another.

What surprised me most was how tightly everything depends on DNS. Before this project I knew DNS mattered. After breaking domain authentication twice because the client was using the wrong DNS server, I understood it differently. DNS is not supporting infrastructure for AD. It is AD. Everything else follows from whether name resolution works.

The commercial side was also something I did not expect to care about and ended up finding interesting. Writing the BOM forced research into actual market pricing for networking hardware in West Africa, which is not the same as European pricing. The pro-forma invoice and payment terms are things you do not think about as a student, but they are what the client actually signs. A technically perfect architecture that does not come with a realistic commercial proposal does not get deployed.

If I did this again I would separate the roles earlier. Running AD DS, DNS, DHCP, WSUS, and Remote Access on one machine is fine for a lab because it forces you to see how each role depends on the others. But a WSUS maintenance window that reboots the server should not also take down authentication for the entire domain. In production those go on different machines.
