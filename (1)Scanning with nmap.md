## Exercise 1: Recconnaissance - From Kali at Ubuntu

### Full Port Scan
command: 
>nmap -p- 192.168.122.209

Result: It showed port 22, SSH, and port 80, HTTP, as being open as well as my MAC address

Ports Open: SSH (22) and HTTP (80)

### Aggressive Scan
command: 
>sudo nmap -A 192.168.122.209

OS Detection: Linux Ubuntu and guessed version ranges of the kernal and system

Other: showed the opened ports as well as ssh-hostkeys, ascertained that the server was Apache2, and tracerouting resulted in one hop

### Vulnerability Scan
command: 
>nmap --script vuln 192.168.122.209

Vulnerabilites found: It couldn't seem to find any vulnerabilities including DOM based XSS, CSRF, and stored XSS

### Learnings
- familiarity with nmap
- -p- scans all ports on target host
- -A  enables an aggressive scan that constitutes OS version, version detection, traceroute, and script scanning
- --script vuln executes scripting engine under "vuln" to find any vulnerabilities on the system

### Link(s)
https://nmap.org/ 
