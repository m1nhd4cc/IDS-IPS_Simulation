# pfSense IDS/IPS Lab with Snort & Suricata

![Network Architecture](./diagrams/network-architecture.png)

A comprehensive lab environment demonstrating a secure network architecture using pfSense. This project showcases the implementation and configuration of an Intrusion Detection System (IDS) with **Snort** and an Intrusion Prevention System (IPS) with **Suricata**.

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Network Architecture](#-network-architecture)
- [Key Features](#-key-features)
- [Directory Structure](#-directory-structure)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation & Configuration](#installation--configuration)
- [How to Test the System](#-how-to-test-the-system)
  - [Testing Snort (IDS)](#testing-snort-ids)
  - [Testing Suricata (IPS)](#testing-suricata-ips)
- [Configuration Files](#-configuration-files)
- [Future Improvements](#-future-improvements)
- [License](#-license)
- [Acknowledgements](#-acknowledgements)

---

## 🚀 Project Overview

This project aims to build a secure, segmented network environment suitable for a small organization. The core of the system is a **pfSense firewall** which handles routing, network segmentation (VLANs, DMZ), and serves as a platform for powerful security packages. We deploy **Snort** to monitor and detect potential threats in IDS mode and **Suricata** in IPS mode to actively block malicious traffic.

This repository serves as a practical guide and a configuration reference for students, network engineers, and security enthusiasts.

---

## 🏗️ Network Architecture

The lab is built on a virtualized environment and segmented into several distinct zones to enforce security policies and isolate critical assets:

* **WAN (Wide Area Network):** Simulates the connection to the Internet.
* **LAN (Local Area Network):** The primary internal user network.
* **DMZ (Demilitarized Zone):** Hosts public-facing services like a Web Server to isolate them from the internal network. * **Management Zone (VÙNG QUẢN TRỊ):** A secure zone for network administrators.
* **Internal Servers Zone (VÙNG MÁY CHỦ NỘI BỘ):** Hosts internal-only servers like Database and Application servers.
* **User Zone (VÙNG NGƯỜI DÙNG):** Segmented into multiple VLANs for different user groups.

---

## ✨ Key Features

* **Firewall & Routing:** Full configuration using pfSense.
* **Network Segmentation:**
    * DMZ for public services.
    * VLANs for user and management zones.
* **Intrusion Detection (IDS):** Using **Snort** to log and alert on suspicious traffic (e.g., ICMP scans).
* **Intrusion Prevention (IPS):** Using **Suricata** to actively block attacks (e.g., large ICMP packets simulating a Ping of Death).
* **Custom Rule Writing:** Examples of custom rules for both Snort and Suricata to detect specific patterns.
* **Port Forwarding:** Exposing the web server in the DMZ to the internet.
* **Dynamic DNS (DDNS):** Using Cloudflare to map a dynamic public IP to a static domain name.

---

## 📁 Directory Structure

```
.
├── configs/
│   ├── pfsense/config-sanitized.xml
│   ├── snort/custom.rules
│   └── suricata/custom.rules
├── diagrams/
│   └── network-architecture.png
├── report/
│   └── Project_Report_II.pdf
├── screenshots/
│   ├── 01-pfsense-setup/
│   ├── ...
└── README.md
```

---

## 🏁 Getting Started

### Prerequisites

* Virtualization Software: **VMware Workstation/Player** or **VirtualBox**.
* pfSense ISO Image (Community Edition).
* Client OS (e.g., Windows 10, Windows Server) for testing.
* XAMPP for setting up the web server in the DMZ.

### Installation & Configuration

The full step-by-step guide is available in the [original report](./report/Project_Report_II.pdf) (Note: The report is in Vietnamese).

The key steps are summarized below, with relevant screenshots in the `screenshots/` directory.

1.  **pfSense Setup:** Install pfSense and configure network interfaces (WAN, LAN, DMZ, etc.).
2.  **DMZ Configuration:** Set up a Windows machine with XAMPP and configure its static IP.
3.  **VLAN Implementation:** Create and configure VLANs for the User and Management zones.
4.  **Firewall Rules:**
    * Create rules to allow/block traffic between zones.
    * Set up Port Forwarding (NAT) to make the DMZ web server accessible from the WAN.
5.  **Snort (IDS) Setup:**
    * Install the Snort package on pfSense.
    * Configure interfaces to monitor.
    * Add custom rules (see `configs/snort/custom.rules`) to detect ping scans.
6.  **Suricata (IPS) Setup:**
    * Install the Suricata package.
    * Enable IPS Mode (Legacy Mode).
    * Add custom rules (see `configs/suricata/custom.rules`) to `DROP` malicious packets.
7.  **(Optional) DDNS Setup:** Configure Dynamic DNS with a provider like Cloudflare.

---

## 🔬 How to Test the System

### Testing Snort (IDS)

From an "attacker" machine on the WAN, ping a machine on the LAN.

```sh
ping 192.168.1.10
```

**Result:** The ping may or may not succeed based on your firewall rules, but Snort will detect it. Check the **Alerts** tab in the Snort service on pfSense. You should see entries matching our custom "ping hehehe" rule.

![Snort Alert Log](./screenshots/04-snort-ids-testing/snort-ping-detection.png)

### Testing Suricata (IPS)

From an "attacker" machine, launch an attack that matches a `DROP` rule, such as a large ICMP packet flood.

```sh
ping 192.168.1.10 -l 65500 -t
```

**Result:** After a few successful pings, Suricata will identify the threat and block the attacker's IP address. The ping command will start showing "Request timed out". You can verify this in the **Blocks** tab in the Suricata service on pfSense.

![Suricata Blocked Host](./screenshots/05-suricata-ips-testing/suricata-blocked-ip.png)

---

## ⚙️ Configuration Files

* **`configs/pfsense/config-sanitized.xml`**: A backup of the pfSense configuration. You can restore from this file in pfSense. **Note:** Sensitive information like MAC addresses and public IPs have been removed.
* **`configs/snort/custom.rules`**: Custom rules for Snort.
* **`configs/suricata/custom.rules`**: Custom rules for Suricata, including `alert` and `drop` actions.

---

## 🔮 Future Improvements

While this lab is a robust demonstration, it can be expanded further:

* **SIEM Integration:** Forward logs from pfSense, Snort, and Suricata to a SIEM (Security Information and Event Management) system like Wazuh or Elastic SIEM for centralized monitoring and correlation.
* **Automation:** Use tools like **Ansible** to automate the configuration of pfSense and the deployment of rules.
* **Advanced IPS Rules:** Implement more sophisticated rulesets (e.g., ET Open) and explore Inline IPS mode with compatible hardware/drivers.
* **Web Application Firewall (WAF):** Add a WAF like HAProxy or ModSecurity to protect the web server in the DMZ.

---

## 📜 License

This project is licensed under the **MIT License**. See the [LICENSE](./LICENSE) file for details.

---

## 🙏 Acknowledgements

* This project was completed as part of my academic coursework.
* Special thanks to the open-source communities behind pfSense, Snort, and Suricata.
