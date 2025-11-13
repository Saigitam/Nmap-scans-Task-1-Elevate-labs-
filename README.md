# 🛡️ Cyber Security Internship - Task 1: Local Network Port Scan

**Internship Program:** Elevate Labs Cyber Security Internship

**Task Objective:** Learn to discover open ports on devices in the local network to understand network exposure.

**Name:** Kothamasu Sai Prasad.

---

## 1. 🛠️ Tools and Command Used (Including TCP SYN Scan Explanation)

| | **Tool** | **Purpose** |
| :--- | :--- | :--- |
| | **Nmap** | Network discovery and security auditing tool. |
| | **Wireshark** | Optional tool for packet analysis (not used for this submission). |

**Nmap Command Executed:**
nmap -sS 192.168.152.0/24

*The command uses the **TCP SYN scan** (`-sS` flag). This scan is a "half-open" technique: Nmap sends a SYN packet, and if the port is open, the target replies with a SYN/ACK. Nmap immediately sends an RST packet to terminate the connection before the full TCP handshake completes. This makes the scan faster and more "stealthy" than a full connect scan.*

---

## 2. 📊 Scan Results Summary (Including Open Port Definition)

The scan of the $192.168.152.0/24$ network identified **2 active hosts** (up).

An **open port** is a logical communication endpoint on a device that is actively listening for incoming network connections. The host $192.168.152.1$ was found to have the following open TCP ports:

### Open Ports Found on Host $192.168.152.1$

| **Port (TCP)** | **Service Name** | **Common Protocol/Use** |
| :---: | :--- | :--- |
| **135** | `msrpc` | Microsoft Remote Procedure Call |
| **139** | `netbios-ssn` | NetBIOS Session Service |
| **445** | `microsoft-ds` | Server Message Block (**SMB**/File Sharing) |
| **902** | `iss-realsecure` | VMware Authentication Service / ESXi Host Agent |
| **912** | `apex-mesh` | VMware Heartbeat / VMware Horizon |
| **3306** | `mysql` | MySQL Database System |

*(The raw Nmap output is attached as `nmapscan.txt`.)*

---

## 3. 🚨 Security Risk Identification (Including Port Scan and TCP/UDP Context)

A **port scan** is the systematic attempt to determine which ports on a host are open. Attackers perform it as the first step of **network reconnaissance** to map services and locate vulnerabilities. The primary security risks identified are:

* **High Risk: SMB/File Sharing Exposure (Ports 139, 445):** These services are high-value targets for malware (e.g., WannaCry) and unauthorized access, particularly if the OS is not patched.
* **High Value Target: Database Compromise (Port 3306):** An exposed MySQL port can be subject to brute-force attacks or exploits, leading to data theft or corruption.
* **Remote Management Vulnerabilities (Ports 135, 902, 912):** Services like RPC and VMware components can be exploited for **Remote Code Execution (RCE)** if the underlying software is outdated.
* **TCP vs. UDP Scanning:** All identified ports use TCP, which is connection-oriented and reliable for scanning. **UDP** (connectionless) scanning is harder because an open port often gives **no response**, making it difficult to distinguish from a filtered port.

---

## 4. 🔒 Mitigation and Security Measures (Including Firewall Role)

To secure the open ports found on $192.168.152.1$, the following measures must be taken. This addresses how open ports can be secured and the role of a firewall:

* **Firewall Enforcement:** Configure a **firewall** to block incoming connections to all unnecessary ports. The **firewall's role** is to act as a crucial security gatekeeper, controlling access to these logical communication endpoints based on predefined rules.
* **Access Restriction:** Restrict access to essential ports (like 3306 and 445) to **only trusted internal IP addresses** (IP whitelisting).
* **Disable Unused Services:** If a service is not actively needed, it should be **disabled entirely** to reduce the attack surface.
* **Patching:** Ensure the operating system and all running services (SMB, MySQL, VMware) are **fully patched** to address known vulnerabilities.

---

## 5. 📝 Wireshark Complementary Role

**Wireshark** is a packet analyzer that captures and displays the raw network traffic. It complements port scanning by allowing a user to **verify the scan results** and troubleshoot firewall rules by observing the exact packets sent and received during the scan, confirming the target's behavior (e.g., seeing the SYN/ACK packets from the host).
