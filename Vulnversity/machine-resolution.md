## Machine Resolution: Vulnversity

[TryHackMe: Vulnversity Machine](https://tryhackme.com/room/vulnversity)

**Objective:** Perform enumeration, exploitation, and privilege escalation on the "Vulnversity" machine to identify vulnerabilities.

---

## Machine Description
The **Vulnversity** machine is an introductory-level machine on TryHackMe that allows practicing basic techniques of enumeration, port scanning, exploitation of common services, and privilege escalation.

## 1. General Information about the Machine
- **Machine Name**: Vulnversity
- **IP Address**: 10.10.120.94
- **Objective**: Gain access and privileges on the machine using ethical hacking techniques.

## 2. Enumeration
We scan with `nmap` to detect service versions. This gives us a better idea of what specific vulnerabilities might be present.
```bash
nmap -sC -sV -oN 10.10.120.94/initial_scan.txt 10.10.120.94
``` 

#### Used flags
- **-sC:** Runs default nmap script scans, getting more details about services (like versions and potential vulnerabilities).
- **-sV:** Detects versions of services on each open port, helping us determine if they are vulnerable or outdated versions.
- **-oN  nmap/initial_scan.txt** Saves the scan results to a file named initial_scan.txt for future reference.

#### Scan Results
```plaintext
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 3.0.3
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
3128/tcp open  http-proxy  Squid http proxy 3.5.12
|_http-server-header: squid/3.5.12
|_http-title: ERROR: The requested URL could not be retrieved
3333/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)

``` 

#### Gobuster
Perform brute force attack with `Gobuster` to determine if there are hidden directories.
When I run the scan like this it shows me an error, this happens because the web server is not on the standard port (port 80), but rather on port 3333
```bash
gobuster dir -u http://10.10.195.20 -w /usr/share/wordlists/dirb/big.txt
``` 

#### Command is executed again specifying port

```bash
gobuster dir -u http://10.10.195.20:3333 -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt
``` 

#### Brute force result
In these results several directories are identified, I focus on one called `/internal/`, in this path I can upload files to the server.
![alt text](imgs/img1.png)

