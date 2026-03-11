# 🏛️ VulnAD-Deployer
An automated "Lab-in-a-Box" for practicing Active Directory exploitation locally.

## 🛠️ Included Vulnerabilities
This lab is intentionally "broken" to simulate real-world Active Directory misconfigurations:
* **Kerberoasting:** A service account (`svc_sql`) is configured with a crackable password and a clear Service Principal Name (SPN).
* **AS-REP Roasting:** A user account (`user_alice`) has Kerberos pre-authentication explicitly disabled.
* **Insecure Shares:** Sensitive data (passwords) is purposely left on an open SMB share accessible by 'Everyone'.
* **Weak Passwords:** Provisioned users have weak, easily guessable passwords (`Password123!`).

## 🚀 Quick Start
### Prerequisites
1. **[Vagrant](https://www.vagrantup.com/downloads)** installed.
2. **[VirtualBox](https://www.virtualbox.org/wiki/Downloads)** installed.
3. The **vagrant-reload** plugin installed (required for handling the AD promotion reboot):
   ```bash
   vagrant plugin install vagrant-reload
   ```

### Spinning up the Lab
1. Clone this repository and navigate to the `vagrant/` directory.
   ```bash
   git clone <repo-url> VulnActiveDirectory-Deployer
   cd VulnActiveDirectory-Deployer/vagrant
   ```
2. Start the Vagrant environment:
   ```bash
   vagrant up
   ```
3. Let the automation finish. The PowerShell provisioning script `Setup-Lab.ps1` will:
   * **Phase 1:** Install AD Domain Services, promote the `Vagrant` VM to a Domain Controller (`vulnerable.local`), and reboot.
   * **Phase 2 (Post-Reboot):** Create the missing users (`admin_bob`, `svc_sql`, `user_alice`), configure the kerberos vulnerabilities, set up the SMB share, and place the `passwords.txt` file inside it.

A Windows 10 workstation (`WS01` at `192.168.56.11`) will also be provisioned, linked via DNS to the Domain Controller.

## 🗺️ Network Topology
- **Domain:** `vulnerable.local`
- **Domain Controller (DC01):** Windows Server 2019 - IP: `192.168.56.10`
- **Workstation (WS01):** Windows 10 - IP: `192.168.56.11`

## 🎯 Learning Objectives
This project bridges the gap between System Administration / Reliability Engineering (SRE) and Offensive Security:
1. **Network Reconnaissance:** Perform AD Enumeration using `Bloodhound` or `PowerView`.
2. **Cracking Kerberos Tickets:** Practice Ticket-based attacks (AS-REProast, Kerberoast) using tools like `Impacket`, `Rubeus`, or `Hashcat`.
3. **Information Disclosure:** Exploit misconfigured, overly-permissive SMB shares to pivot laterally.
4. **Defensive Knowledge:** Understand how these attacks actually map to configurations like `DONT_REQ_PREAUTH` and `.ServicePrincipalNames`, and how to remediate them in a production environment.

## 📝 Disclaimer
🚨 **This project is for educational purposes only.** The configurations created here are intentionally insecure and extremely dangerous. Do not deploy this in a production environment or expose these VMs to the internet! Use internally on your local hypervisor.
