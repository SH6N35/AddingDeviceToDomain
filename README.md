# Windows 11 Client Enterprise Domain Join

## 🎯 Objective
Configure, secure, and join a virtualized Windows 11 client workstation to an on-premises Active Directory domain (`ApexTech.com`) to establish centralized identity management, security policies, and domain authentication.

---

## 🛠️ Environment & Architecture
- **Hypervisor:** Ubuntu Linux (KVM / QEMU) via Cockpit (VirtIO drivers enabled)
- **Domain Controller:** Windows Server 2022 (`ApexTech.com` at Static IP `192.168.122.22`)
- **Client Endpoint:** Windows 11 Pro (UEFI, vTPM 2.0, Hostname: `WIN11-Client01`)

---

## 📋 Implementation Steps

### 1. Client OS Setup & Local Account Baseline
- Deployed a new Windows 11 Pro virtual machine in KVM with 4 vCPUs, 4 RAM, and VirtIO storage/network controllers.
- Installed VirtIO network drivers (`netkvm`) via the mounted ISO to bring the virtual network adapter online.
- Bypassed online Microsoft account requirements during OOBE (using the invalid email trick or local network isolation) to establish a clean local administrator account baseline.

### 2. Network & DNS Configuration (Crucial Step)
- Navigated to **Settings > Network & internet > Ethernet** and configured manual IPv4 settings.
- Set the **Preferred DNS Server** explicitly to point to the Active Directory Domain Controller (`192.168.122.22`), ensuring internal DNS queries route correctly without relying on external public resolvers like `8.8.8.8`.
- Verified network reachability and name resolution by running `ping 192.168.122.22` and `ping ApexTech.com` in Command Prompt.

### 3. Executing the Domain Join
- Configured a clean computer hostname (`WIN11-Client01`) via **Settings > System > About** and restarted the VM[cite: 5].
- Navigated to **Settings > System > About > Domain or workgroup**, clicked **Change**, selected **Domain**, and entered the enterprise domain name: `ApexTech.com`[cite: 5].
- Authenticated using Domain Administrator credentials (`APEXTECH\Administrator`) when prompted by the security dialogue.
- Successfully received the *"Welcome to the ApexTech.com domain"* confirmation prompt and initiated a system reboot[cite: 5].

---

## ✅ Verification & Testing

1. **Domain User Authentication:**
   - At the post-reboot login screen, selected **Other user** and signed in using domain credentials (`APEXTECH\Username`). Confirmed successful creation of the local domain user profile.

2. **Active Directory Verification & OU Placement:**
   - Logged into the Domain Controller and opened **Active Directory Users and Computers (`dsa.msc`)**[cite: 2].
   - Located the computer object under the default **Computers** container and successfully migrated it into the custom **Workstations** Organizational Unit (OU) to ensure targeted Group Policy enforcement[cite: 2].

---

## 📸 Lab Evidence
*(Add screenshots of your Windows 11 system domain properties showing "ApexTech.com" and Active Directory Users and Computers showing the computer object inside the Workstations OU here)*
