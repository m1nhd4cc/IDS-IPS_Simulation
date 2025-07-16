# pfSense IDS/IPS Lab with Snort & Suricata
**(Xây dựng hệ thống Mạng An toàn với pfSense, Snort & Suricata)**

[![pfSense 2.7.2](https://img.shields.io/badge/pfSense-2.7.2-red.svg)](https://www.pfsense.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

[English](#english-version) | [Tiếng Việt](#phiên-bản-tiếng-việt)

---

### **English Version**

A repository providing the configuration and methodology for building a complete network security lab environment. This project uses **pfSense** as the core firewall, **Snort** for Intrusion Detection (IDS), and **Suricata** for Intrusion Prevention (IPS).

### 🚨 **Disclaimer**
**This project is intended for educational and research purposes only.** It should only be deployed in controlled, isolated lab environments (e.g., using VMware, VirtualBox). The author is not responsible for any misuse or damage caused by the configurations or information provided herein. Unauthorized network activities are illegal.

### **Features / Key Technologies**

* **Firewall & Router:** pfSense
* **Network Segmentation:** Configuration for DMZ, VLANs, LAN, and WAN zones.
* **Intrusion Detection System (IDS):** Deploys **Snort** with custom rules to detect and log suspicious activity like ICMP scans
* **Intrusion Prevention System (IPS):** Deploys **Suricata** in IPS mode with rules to actively `DROP` malicious traffic, such as a simulated Ping of Death attack.
* **Web Services:** A sample web server using XAMPP is placed in the DMZ for secure public access.
* **Dynamic DNS (DDNS):** Configuration using Cloudflare to map a dynamic public IP to a static domain name.

### **Setup & Prerequisites**

* **Virtualization:** VMware Workstation / VirtualBox
* **Software:**
    * pfSense ISO (Community Edition) 
    * A client OS (e.g., Windows 10, Windows Server) for servers and user machines
    * XAMPP for the web server.

### **How to Use & Test**

1.  **Clone the repository:**
    ```bash
    git clone <your-repo-url>
    ```
2.  **Setup the Lab:**
    * The entire pfSense configuration can be restored from the `configs/pfsense/config-sanitized.xml` file.
    * For a detailed, step-by-step guide, please refer to the [full project report](./report/Project_Report_II.pdf).

3.  **Test the IDS (Snort):**
    * **Action:** From an attacker machine, ping a host inside the LAN.
    * **Expected Result:** Navigate to **Services > Snort > Alerts** in pfSense. An alert will be logged, matching the custom rule for ICMP detection.

4.  **Test the IPS (Suricata):**
    * **Action:** From an attacker machine, launch a large packet flood to simulate a DoS attack.
        ```sh
        ping 192.168.1.10 -l 65500 -t
        ```
    * **Expected Result:** The attack will be stopped after a few packets (`Request timed out`). Navigate to **Services > Suricata > Blocks** to see the attacker's IP address being actively blocked.

### **Project Report**
For a detailed theoretical background, methodologies, and results, please refer to the full project report **[here](./report/Project_Report_II.pdf)**.

### **Authors & Acknowledgments**
* **Author:** *Dang Minh Duc*
* This project was developed for academic purposes. Special thanks to the open-source communities behind pfSense, Snort, and Suricata.

---

### **Phiên bản Tiếng Việt**

Đây là repository cung cấp cấu hình và phương pháp luận để xây dựng một môi trường lab an ninh mạng hoàn chỉnh. Project sử dụng **pfSense** làm tường lửa trung tâm, **Snort** cho việc Phát hiện Xâm nhập (IDS), và **Suricata** cho việc Ngăn chặn Xâm nhập (IPS).

### 🚨 **Trách nhiệm**
**Project này chỉ dành cho mục đích giáo dục và nghiên cứu khoa học.** Chỉ nên được triển khai trong môi trường lab được kiểm soát và cô lập (ví dụ: VMware, VirtualBox). Tác giả không chịu trách nhiệm cho bất kỳ việc lạm dụng hoặc thiệt hại nào gây ra bởi các cấu hình hoặc thông tin được cung cấp. Mọi hành vi trái phép trên mạng là vi phạm pháp luật.

### **Các tính năng / Công nghệ chính**

* **Tường lửa & Router:** pfSense
* **Phân vùng mạng:** Cấu hình cho các vùng DMZ, VLANs, LAN và WAN.
* **Hệ thống Phát hiện Xâm nhập (IDS):** Triển khai **Snort** với các rule tùy chỉnh để phát hiện và ghi log các hoạt động đáng ngờ như quét ICMP.
* **Hệ thống Ngăn chặn Xâm nhập (IPS):** Triển khai **Suricata** ở chế độ IPS với các rule để chủ động `DROP` (chặn) lưu lượng độc hại, ví dụ như một cuộc tấn công Ping of Death giả lập.
* **Dịch vụ Web:** Một web server mẫu sử dụng XAMPP được đặt trong vùng DMZ để đảm bảo truy cập công cộng an toàn.
* **Dynamic DNS (DDNS):** Cấu hình sử dụng Cloudflare để ánh xạ một địa chỉ IP công cộng động tới một tên miền tĩnh.

### **Cài đặt & Yêu cầu**

* **Nền tảng ảo hóa:** VMware Workstation / VirtualBox
* **Phần mềm:**
    * File ISO của pfSense (Bản Community)
    * Hệ điều hành máy khách (ví dụ: Windows 10, Windows Server) để làm máy chủ và máy người dùng.
    * XAMPP cho web server.

### **Hướng dẫn sử dụng & Kiểm tra**

1.  **Clone repository về máy:**
    ```bash
    git clone <your-repo-url>
    ```
2.  **Thiết lập môi trường Lab:**
    * Toàn bộ cấu hình pfSense có thể được khôi phục từ file `configs/pfsense/config-sanitized.xml`.
    * Để xem hướng dẫn chi tiết từng bước, vui lòng tham khảo **[báo cáo đầy đủ của đề tài](./report/Project_Report_II.pdf)**.

3.  **Kiểm tra IDS (Snort):**
    * **Hành động:** Từ một máy attacker, thực hiện ping đến một máy trong mạng LAN.
    * **Kết quả mong đợi:** Truy cập vào pfSense, vào mục **Services > Snort > Alerts**. Hệ thống sẽ ghi nhận log cảnh báo khớp với rule tùy chỉnh đã tạo để phát hiện ICMP.

4.  **Kiểm tra IPS (Suricata):**
    * **Hành động:** Từ máy attacker, thực hiện một cuộc tấn công bằng gói tin lớn để giả lập tấn công DoS.
        ```sh
        ping 192.168.1.10 -l 65500 -t
        ```
    * **Kết quả mong đợi:** Cuộc tấn công sẽ bị chặn sau vài gói tin (hiển thị `Request timed out`). Truy cập **Services > Suricata > Blocks** để thấy địa chỉ IP của kẻ tấn công đang bị chặn.

### **Báo cáo Đề tài**
Để xem chi tiết về cơ sở lý thuyết, phương pháp thực hiện và kết quả, vui lòng tham khảo báo cáo đầy đủ của đề tài **[tại đây](./report/Project_Report_II.pdf)**.

### **Tác giả & Lời cảm ơn**
* **Tác giả:** *Dang Minh Duc*
* Project này được phát triển cho mục đích học tập. Xin gửi lời cảm ơn đặc biệt đến cộng đồng mã nguồn mở của pfSense, Snort và Suricata.
