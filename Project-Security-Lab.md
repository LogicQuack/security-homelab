# Project: Multi-VM Security Lab Environment

## Objective
Build a multi-VM environment where I can practice both attack and defense 

## Lab Architecture

**VMs:**
- Kali Linux 2025.3 (Attacker/Scanner)
- Ubuntu Server (Web services target)
- Rocky Linux 9 (Hardening practice target)


---

## VM Configurations

## Rocky Linux (Headless)
- **Platform:** UTM on MacBook Air M2
- **Specs:** Mem: 4 GB, Size: 20GB, CPUs: 2
- **IP Address:** 192.168.64.2
- **Purpose:** Hardening as well as practice target
- **Status:**  Operational

**Setup Challenges (check documentation for more):**
- Initial boot/display issues with VirtualBox + Rocky 8
- Solution: Switched to UTM + Rocky 9, configured as headless

### Kali Linux
- **Platform:** QEMU/virt-manager on CachyOS
- **Specs:** Mem: 4 GB, Size: 30, CPUs: 2
- **IP Address:** 192.168.122.207
- **Purpose:** Attack vector
- **Status:** Operational

**Setup Challenges (check documentation for more):**
- Booting problems
- Network connectivity failure
- Solution: reconfigured network with iptables backend and switching configuration when initially setting up in VMM

### Ubuntu Server
- **Platform:** QEMU/virt-manager on CachyOS
- **Specs:** 25GB storage, 5GB RAM, 2 CPU cores
- **IP Address:** 192.168.122.209
- **Purpose:** Target for pentesting as well as tpo serve as the Apache server of my "network"
- **Status:** Operational

**Installed Services:**
- Apache2 (HTTP server) - Port 80
- OpenSSH server - Port 22

---

## Week 1 Activities

### Network Reconnaissance
**Date:** 12-15-2025

**Objective:** Perform initial network scan to identify services on Ubuntu target

**Tools:** Nmap

**Commands:**
```bash
nmap -sV 192.168.122.209
```

**Results:**
```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH [version]
80/tcp open  http    Apache httpd [version]
```

**Findings:**
- Two services exposed: SSH and HTTP
- Both services using default ports
- No firewall rules blocking access

**Personal Learning:**
- differentiation between specific port scanning with -p and -sV

---

## Skills Demonstrated
- Virtual machine deployment and configuration
- Linux system administration (package management, service control)
- Network troubleshooting (DHCP, firewall, libvirt networking)
- Penetration testing reconnaissance (Nmap)
- Technical documentation

## Tools & Technologies Used
- **Virtualization:** QEMU/KVM, virt-manager, UTM
- **Operating Systems:** Kali Linux, Ubuntu Server, Rocky Linux
- **Networking:** libvirt, iptables, nmcli
- **Security Tools:** Nmap, DVWA
- **Services:** Apache2, OpenSSH

---

## Current Progress: 
- **finished exercise 1, see "scanning with nmap" for details**

## Next Steps
- [ ] Install vulnerable web application (DVWA)
- [ ] Attack Weak DVWA environment
- [ ] Set up basic firewall rules on Ubuntu
- [ ] Document attack surface analysis

---

## References
- Kali Linux Documentation: https://www.kali.org/docs/
- Nmap Reference Guide: https://nmap.org/book/man.html
- Ubuntu Server Guide: https://ubuntu.com/server/docs
