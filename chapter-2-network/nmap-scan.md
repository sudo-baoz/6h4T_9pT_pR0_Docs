# 📡 Mission 10: Nmap - Vua của sự rà quét

### (The God of Network Reconnaissance)

> **📜 Mission Brief:** Trong không gian mạng, thông tin là quyền lực. **Nmap** không chỉ là một cái máy quét (scanner), nó là một hệ sinh thái trinh sát toàn diện.
>
> *Từ Hollywood (The Matrix) đến phòng Lab của FBI, Nmap là tiêu chuẩn bắt buộc.*

-----

## 🛑 PROTOCOL ZERO: CẢNH BÁO PHÁP LÝ (LEGAL WARNING)

Trước khi kích hoạt bất kỳ lệnh nào, hãy khắc cốt ghi tâm:

> ⚠️ **CẢNH BÁO ĐỎ:** Hành vi sử dụng Nmap quét vào IP/Domain mà bạn **KHÔNG SỞ HỮU** hoặc **KHÔNG CÓ VĂN BẢN CHO PHÉP** được coi là **TẤN CÔNG MẠNG**.
>
>   * Hậu quả: Bị ISP cắt mạng, bị quản trị viên ban IP vĩnh viễn, hoặc truy tố trách nhiệm hình sự.
>   * **Phạm vi thực hành:** Chỉ quét `localhost`, máy ảo (VMware/VirtualBox) của bạn, hoặc `scanme.nmap.org`.

-----

## ⚙️ MODULE 1: CƠ CHẾ HOẠT ĐỘNG (THE ENGINE)

Tại sao Nmap lại mạnh đến thế? Vì nó không chỉ "gõ cửa". Nó gửi đi các gói tin được chế tác đặc biệt và phân tích cách server phản hồi (hoặc im lặng).

### 1\. Host Discovery (Ping Sweep)

Trước khi quét cổng, Nmap hỏi: "Máy này có sống không?".

  * Mặc định: Gửi ICMP Echo Request + TCP SYN đến port 443 + TCP ACK đến port 80.
  * **Vấn đề:** Firewall hiện đại thường chặn ICMP (Ping).
  * **Giải pháp Hacker:** Dùng cờ `-Pn`.
      * **`-Pn` (Treat all hosts as online):** Ra lệnh cho Nmap: *"Đừng quan tâm nó có ping được không, cứ quét cổng đi\!"*. Đây là lệnh bắt buộc khi quét server Windows bật Firewall.

### 2\. Port States (Trạng thái Cổng)

Nmap không chỉ báo Mở/Đóng. Nó báo 3 trạng thái quan trọng:

1.  **🟢 OPEN:** Có ứng dụng đang lắng nghe. → **MỤC TIÊU TẤN CÔNG.**
2.  **🔴 CLOSED:** Gói tin đến nơi nhưng không có ứng dụng nào nhận, server gửi trả gói `RST` (Reset). → Máy đang bật, nhưng cổng này rảnh.
3.  **FILTERED:** Gói tin bị nuốt chửng. Không có hồi âm. → **Có Firewall/IPS đang chặn.**

-----

## ⚔️ MODULE 2: KỸ THUẬT QUÉT NÂNG CAO (SCANNING TECHNIQUES)

### A. TCP SYN Scan (`-sS`) - "Kẻ Tàng Hình"

Đây là kỹ thuật mặc định khi chạy với `sudo`. Nó nhanh và ít để lại dấu vết.

  * **Quy trình (Half-open scanning):**
    1.  **Hacker:** Gửi `SYN` (Xin kết nối).
    2.  **Server:** Gửi `SYN/ACK` (Đồng ý).
    3.  **Hacker:** Gửi `RST` (Hủy kèo ngay lập tức).
  * **Tại sao hay?** Vì kết nối chưa hoàn tất, ứng dụng (như Apache/MySQL) thường không ghi vào file Log. Chỉ có Firewall mức thấp mới phát hiện được.

### B. TCP Connect Scan (`-sT`) - "Kẻ Lịch Sự"

Dùng khi bạn không có quyền `sudo` (non-root user).

  * **Quy trình:** Hoàn thành đủ 3 bước bắt tay (SYN → SYN/ACK → ACK). Kết nối được thiết lập xong rồi mới ngắt.
  * **Nhược điểm:** Rất ồn ào. Log của server sẽ ghi lại IP của bạn ngay lập tức.

### C. UDP Scan (`-sU`) - "Kẻ Kiên Nhẫn"

Dùng để tìm DNS (53), DHCP (67), NTP (123).

  * **Đặc điểm:** Gửi gói tin đi và chờ. Nếu server im lặng → Có thể mở hoặc bị lọc. Nmap phải chờ timeout nên quét kiểu này **CỰC KỲ LÂU**.

-----

## 🧠 MODULE 3: SERVICE & OS FINGERPRINTING

Biết cổng mở là chưa đủ. Bạn phải biết **đích danh** kẻ đang đứng sau cổng đó.

### 1\. Service Version Detection (`-sV`)

Nmap sẽ kết nối vào cổng và "nói chuyện" với dịch vụ để lấy Banner.

  * *Ví dụ:* Thay vì báo "Port 80 Open", nó báo "Apache httpd 2.4.49".
  * **Giá trị:** Bạn lên Google tìm "Apache 2.4.49 vulnerability" → Thấy lỗi **Path Traversal** → Khai thác!

### 2\. OS Detection (`-O`)

Nmap đo đạc các thông số nhỏ nhặt của gói tin TCP/IP (như TTL, Window Size, ECN bit) để đoán hệ điều hành.

  * Kết quả: "Running: Linux 4.X | 5.X" hoặc "OS: Windows 10".

-----

## 🤖 MODULE 4: NMAP SCRIPTING ENGINE (NSE) - "VŨ KHÍ TỐI THƯỢNG"

Đây là tính năng biến Nmap từ "máy quét" thành "máy hack". Nmap tích hợp sẵn ngôn ngữ lập trình **Lua** để tự động hóa việc tìm lỗ hổng.

Các file script (`.nse`) nằm ở `/usr/share/nmap/scripts/`.

### Các nhóm script phổ biến:

  * `default` (`-sC`): Chạy các script cơ bản, an toàn.
  * `vuln`: Tự động dò tìm các lỗ hổng đã biết (CVE). **(Cực mạnh nhưng dễ bị chặn)**.
  * `auth`: Thử dò username/password mặc định.
  * `safe`: Chỉ lấy thông tin, không gây hại.

**Ví dụ lệnh hủy diệt:**

```bash
nmap -p 445 --script=smb-vuln-ms17-010 192.168.1.5
```

*Lệnh này kiểm tra xem máy mục tiêu có dính lỗi **EternalBlue** (WannaCry) hay không.*

-----

## ⚡ MODULE 5: PERFORMANCE & OUTPUT (HIỆU SUẤT)

### 1\. Timing Templates (`-T0` đến `-T5`)

Điều chỉnh tốc độ quét.

  * `-T0` (Paranoid): Siêu chậm để trốn IDS. (Quét 1 cổng mất 5 phút).
  * `-T3` (Normal): Mặc định.
  * `-T4` (Aggressive): **Khuyên dùng.** Nhanh và ổn định cho mạng LAN/Wifi tốt.
  * `-T5` (Insane): Quá nhanh, dễ làm sập mạng hoặc bỏ sót cổng.

### 2\. Xuất báo cáo (`-oA`)

Làm Hacker là phải biết viết báo cáo (Report). Đừng chỉ nhìn màn hình rồi tắt.

  * `-oN result.txt`: Lưu định dạng text thường.
  * `-oX result.xml`: Lưu XML để import vào Metasploit.
  * `-oA result_folder`: Lưu tất cả các định dạng.

-----

## 🎯 GRAND LAB: THE ULTIMATE SCAN

Hãy kết hợp tất cả kiến thức trên vào một câu lệnh chuẩn mực (Best Practice) để quét máy chủ `scanme.nmap.org`.

### Bước 1: Lên đạn (Cấu trúc lệnh)

```bash
sudo nmap -sS -sV -sC -O -T4 -p- -oN scan_report.txt scanme.nmap.org
```

### Bước 2: Giải phẫu câu lệnh

| Tham số | Ý nghĩa chiến thuật |
| :--- | :--- |
| `sudo` | Chạy quyền Root để dùng SYN Scan (`-sS`) và OS Detect (`-O`). |
| `-sS` | **Stealth Scan:** Quét ẩn, khó bị phát hiện. |
| `-sV` | **Version:** Hiện phiên bản phần mềm chi tiết. |
| `-sC` | **Script:** Chạy các script mặc định (tương đương `--script=default`). |
| `-O` | **OS:** Đoán hệ điều hành. |
| `-T4` | **Timing:** Tốc độ cao. |
| `-p-` | **All Ports:** Quét toàn bộ 65,535 cổng (thay vì chỉ 1000 cổng đầu). |
| `-oN ...` | **Output:** Lưu kết quả vào file `scan_report.txt`. |

### Bước 3: Phân tích kết quả (Sample Output)

```text
Starting Nmap 7.92 ...
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.20s latency).

PORT      STATE    SERVICE      VERSION
22/tcp    open     ssh          OpenSSH 6.6.1p1 Ubuntu (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 ac:00:a0:1a:82:ff:cc:55... (DSA)
|_  2048 20:24:b6:ca:9c:61:68:5e... (RSA)

80/tcp    open     http         Apache httpd 2.4.7 ((Ubuntu))
|_http-title: Go ahead and ScanMe!
|_http-server-header: Apache/2.4.7 (Ubuntu)

Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
```

> **🕵️ Nhận định:**
>
> 1.  **Mục tiêu:** Chạy Linux Kernel 3.x hoặc 4.x.
> 2.  **Web (Port 80):** Chạy Apache 2.4.7 (Phiên bản này ra mắt năm 2013 → Rất cũ → **Tiềm năng khai thác cao**).
> 3.  **SSH (Port 22):** Đang mở, có thể thử Brute-force nếu password yếu.

-----

### 🏆 MISSION COMPLETE

Bạn đã nắm trong tay thanh gươm sắc bén nhất của giới bảo mật.