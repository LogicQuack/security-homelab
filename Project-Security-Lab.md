# Project: Multi-VM Security Lab Environment

## Objective
Build a multi-VM environment where I can practice both attack and defense 

## Lab Architecture

**VMs:**
- Kali Linux 2025.3 (Attacker/Scanner)
- Ubuntu Server (Web services target)
---

## VM Configurations

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
  
## References
- Kali Linux Documentation: https://www.kali.org/docs/
- Nmap Reference Guide: https://nmap.org/book/man.html
- Ubuntu Server Guide: https://ubuntu.com/server/docs
