# Hybrid Network Infrastructure Deployment: Linux & Windows Server

![Debian](https://img.shields.io/badge/Debian-A81D33?style=for-the-badge&logo=debian&logoColor=white) ![Windows Server](https://img.shields.io/badge/Windows_Server-0078D6?style=for-the-badge&logo=windows&logoColor=white) ![Apache](https://img.shields.io/badge/Apache-D22128?style=for-the-badge&logo=Apache&logoColor=white) ![Bash](https://img.shields.io/badge/Bash-4EAA25?style=for-the-badge&logo=GNU%20Bash&logoColor=white)

## Overview
This project demonstrates the design and deployment of a fully functional hybrid network infrastructure utilizing hardware virtualization. It integrates a **Debian (Linux)** server handling critical web and file-sharing services, with a **Windows Server** managing network addressing and name resolution. 

The architecture is built with a strong focus on high availability, data redundancy, and secure access control, simulating a real-world enterprise production environment.

## Architecture & Topology
*   **Virtualization Platform:** Hyper-V
*   **Linux Node (Debian):** Hosts Apache, vsftpd, and OpenSSH. Equipped with a software-based RAID 1 array for data resilience.
*   **Windows Node (Windows Server):** Acts as the network controller, hosting DHCP and DNS services for the local subnet.

## Tech Stack & Key Implementations

### 1. Data Redundancy & Storage Management (Debian)
*   Implemented a **RAID 1** (Mirroring) array using `mdadm` across multiple virtual disks to ensure data integrity and fault tolerance for the file server.
*   Formatted the array with the `ext4` file system and configured persistent mounting via `/etc/fstab`.
*   Successfully simulated and resolved a drive failure, demonstrating automatic array reconstruction capabilities.

### 2. Network Services (Windows Server)
*   **DHCP Server:** Configured an IPv4 scope (`192.168.20.0/26`) with defined exclusions, lease durations, and default gateway assignments to automate client provisioning.
*   **DNS Server:** Established Forward and Reverse Lookup Zones. Created `A` and `CNAME` records to route internal traffic (e.g., `www.hybridnetwork.com` and `ftp.hybridnetwork.com`) to the correct virtual nodes.

### 3. Server Hardening & Secure Access (Debian)
*   **SSH Configuration:** Hardened remote access by modifying the default OpenSSH port (from `22` to `499`) and strictly prohibiting `root` login.
*   **Firewalling:** Implemented `ufw` to restrict incoming traffic, opening only specific ports required for HTTP, HTTPS, FTP, and the custom SSH port.
*   **FTP Access Control:** Configured `vsftpd` to disable anonymous access, restricting FTP login to authorized local users with explicit read/write permissions mapped to the RAID 1 mount point.

## Proof of Concept & Testing
The deployment was validated using a Windows 10 client machine within the same virtual network[cite: 1]. Successful test cases included:
1.  Dynamic IP allocation and DNS suffix assignment via DHCP (`ipconfig /renew`).
2.  Successful hostname resolution via ICMP (`ping www.hybridnetwork.com`).
3.  Authenticated read/write file transfers via FTP to the RAID array.
4.  Successful rendering of the Apache-hosted internal webpage.

---

### Notes for the Repository
*   *Screenshots showcasing the RAID array reconstruction, FTP access, and DNS resolution have been archived in the `assets/` directory as proof of concept.*
*   *The original technical documentation and configuration logs are available in the `docs/` folder.*