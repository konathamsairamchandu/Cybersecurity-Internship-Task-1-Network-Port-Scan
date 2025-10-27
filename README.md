# Cybersecurity Internship Task 1: Network Port Scan

This repository contains the deliverables for Task 1, which involves scanning a local network to identify open ports and potential security risks.

## Objective
The goal was to learn basic network reconnaissance by scanning my local network with Nmap, identifying open ports and services, and analyzing the potential security exposure.

## Tools Used
* **Zenmap (Nmap GUI):** The tool used for running the scan and viewing the results.

## Process
1.  **IP Discovery:** I identified my local network range as `192.168.1.0/24`.
2.  **Scanning:** I performed a TCP SYN (`-sS`) scan using the Nmap command within Zenmap and saved the output. The command was:
    ```
    nmap -sS -oN scan_results.txt 192.168.1.0/24
    ```
3.  **Analysis:** I analyzed the scan output to research the services on each open port and evaluate the security risks.

---

## Scan Results
The full, raw scan output is available in the `scan_results.txt` file in this repository.

### Scan Analysis & Risk Assessment

My scan identified two active hosts on the network.

**Host: `192.168.1.1` (Router)**

* **Open Ports Found:**
    * `21/tcp (ftp)`
    * `22/tcp (ssh)`
    * `23/tcp (telnet)`
    * `53/tcp (domain)`
    * `80/tcp (http)`
    * `443/tcp (https)`
* **Analysis:** This is the network router. It has several high-risk ports open.
* **Identified Risks:**
    * **Critical Risk:** `23/tcp (telnet)` is open. This is an unencrypted remote login protocol. An attacker could easily sniff the admin password.
    * **High Risk:** `21/tcp (ftp)` is open. This is an unencrypted file transfer protocol. Any login credentials would be sent in plain text.
    * **Medium Risk:** `80/tcp (http)` is open for the admin panel, which is unencrypted.
* **Remediation:** I need to log in to the router's admin panel (using the secure `https` port) and find the settings to **disable Telnet** and **disable FTP**. I should also disable HTTP access and only allow HTTPS.

**Host: `192.168.1.2` (Windows PC)**

* **Open Ports Found:**
    * `135/tcp (msrpc)`
    * `139/tcp (netbios-ssn)`
    * `445/tcp (microsoft-ds)`
    * `2179/tcp (vmrdp)`
* **Analysis:** This is a Windows device, likely running VMware virtualization software.
* **Identified Risks:**
    * **High Risk:** `445/tcp (microsoft-ds)` is open. This is the Windows File Sharing port (SMB) and is a primary target for malware and worms (like WannaCry) to spread across a network.
* **Remediation:** Unless file sharing is absolutely necessary, it should be disabled in Windows settings. If it is needed, a strong password and an up-to-date, patched operating system are essential. The Windows host firewall should be configured to restrict access to this port.
