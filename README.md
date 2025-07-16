# pfSense IDS/IPS Lab with Snort & Suricata

![Network Architecture](./diagrams/network-architecture.png)

Repository này cung cấp cấu hình và hướng dẫn để xây dựng một môi trường lab an ninh mạng hoàn chỉnh, sử dụng **pfSense** làm tường lửa trung tâm, **Snort** cho việc phát hiện xâm nhập (IDS), và **Suricata** cho việc ngăn chặn xâm nhập (IPS).

---

### Kiến trúc & Công nghệ chính

* **Tường lửa:** pfSense
* **Phân vùng mạng:** DMZ, VLANs, LAN, WAN
* **IDS (Phát hiện):** Snort với các custom rule để phát hiện gói tin ICMP.
* **IPS (Ngăn chặn):** Suricata với các custom rule để `DROP` (chặn) các gói tin tấn công.
* **Web Server:** XAMPP trong vùng DMZ.
* **DDNS:** Cấu hình với Cloudflare.

---

### Cấu trúc thư mục

```
.
├── configs/            # Chứa file cấu hình cho pfSense, Snort, Suricata
├── diagrams/           # Chứa sơ đồ kiến trúc mạng
├── report/             # Chứa file báo cáo gốc (tiếng Việt)
├── screenshots/        # Chứa ảnh chụp màn hình các bước quan trọng
└── README.md
```

---

### Thiết lập & Kiểm tra

#### Cài đặt

Toàn bộ cấu hình có thể được khôi phục từ file `configs/pfsense/config-sanitized.xml`. Để xem hướng dẫn chi tiết từng bước, vui lòng tham khảo [báo cáo gốc](./report/Project_Report_II.pdf).

#### Kiểm tra IDS (Snort)

1.  **Hành động:** Từ máy attacker, thực hiện ping tới một máy trong mạng LAN.
    ```sh
    ping 192.168.1.10
    ```
2.  **Kết quả:** Truy cập vào pfSense, vào mục **Services > Snort > Alerts**. [cite_start]Hệ thống sẽ ghi nhận log cảnh báo phát hiện gói tin ICMP. [cite: 712]

#### Kiểm tra IPS (Suricata)

1.  **Hành động:** Từ máy attacker, thực hiện tấn công "Ping of Death" để làm quá tải hệ thống.
    ```sh
    ping 192.168.1.10 -l 65500 -t
    ```
2.  [cite_start]**Kết quả:** Sau vài gói tin, các gói ping sẽ bị `Request timed out`[cite: 1081]. [cite_start]Truy cập pfSense, vào mục **Services > Suricata > Blocks**, IP của máy attacker sẽ bị chặn. [cite: 1196]

---

### License

Dự án này được cấp phép dưới **Giấy phép MIT**.
