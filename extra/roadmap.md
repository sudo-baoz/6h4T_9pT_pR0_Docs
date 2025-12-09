# 🗺️ Roadmap: Lộ Trình Từ Zero Đến Hero

> **Mục tiêu:** Biến một người chưa biết gì về bảo mật trở thành một tay chơi CTF có nền tảng vững chắc.
> **Thời gian ước tính:** 3 - 6 tháng (Tùy độ "cày" của bạn).



---

## 🏁 Stage 0: Setup & Mindset (Khởi động)
**"Chuẩn bị hành trang trước khi ra khơi."**

Trước khi gõ lệnh, bạn cần công cụ và tư duy đúng đắn.
* **Mindset:**
    * [ ] **Đạo đức:** Hacker Mũ Trắng (White Hat). Không phá hoại, chỉ tấn công khi được phép.
    * [ ] **Tự học:** Kỹ năng quan trọng nhất là "Google Dorking" (Biết cách tìm kiếm lỗi).
    * [ ] **Kiên trì:** "Try Harder" - Không bỏ cuộc khi gặp lỗi.
* **Setup:**
    * [ ] Cài đặt **Kali Linux** (Máy ảo VMWare/VirtualBox hoặc Dual Boot).
    * [ ] Làm quen với Terminal (Màn hình đen).
    * [ ] Tham gia cộng đồng (Discord, Cookie Arena, Reddit).

---

## 🐧 Stage 1: Linux Survival (Kỹ năng sinh tồn)
**"Làm chủ ngôi nhà của Hacker."**

Hầu hết các server trên thế giới chạy Linux. Bạn không thể hack thứ bạn không biết dùng.
* **Command Line (CLI):**
    * [ ] Điều hướng: `cd`, `ls`, `pwd`.
    * [ ] Thao tác file: `cat`, `cp`, `mv`, `rm`, `mkdir`.
    * [ ] Quyền hạn: `chmod`, `chown`, `sudo`.
* **Tools cơ bản:**
    * [ ] Soạn thảo: `nano` hoặc `vim`.
    * [ ] Lọc dữ liệu: `grep`, `pipe (|)`, `redirection (>)`.
    * [ ] Cài phần mềm: `apt`, `git`.

> **🎯 Mục tiêu:** Có thể dùng Linux cả ngày mà không cần chạm vào chuột.

---

## 🌐 Stage 2: Networking (Bản đồ mạng lưới)
**"Hiểu đường đi nước bước của dữ liệu."**

Internet không phải phép thuật, nó là các gói tin.
* **Lý thuyết:**
    * [ ] Mô hình **OSI** & **TCP/IP**.
    * [ ] IP (Public vs Private), Subnet Mask, MAC Address.
    * [ ] Các cổng phổ biến (Port): 21, 22, 53, 80, 443.
    * [ ] Giao thức: TCP (Bắt tay 3 bước), UDP, ICMP, DNS.

* **Vũ khí:**
    * [ ] **Nmap:** Quét cổng, tìm dịch vụ.
    * [ ] **Wireshark:** Bắt và phân tích gói tin.
    * [ ] **Netcat:** Tạo kết nối, chat, truyền file.

---

## 🕷️ Stage 3: Web Exploitation (Cánh cửa phổ biến nhất)
**"Nơi tập trung 80% các cuộc tấn công."**

Web là mảng rộng nhất và dễ tiếp cận nhất.
* **Kiến thức:**
    * [ ] Giao thức HTTP/HTTPS (Request, Response, Header).
    * [ ] Cookie & Session.
    * [ ] HTML/JavaScript cơ bản.
* **OWASP Top 10 (Các lỗ hổng kinh điển):**
    * [ ] **SQL Injection (SQLi):** Tấn công cơ sở dữ liệu.
    * [ ] **XSS (Cross-Site Scripting):** Tấn công người dùng.
    * [ ] **Command Injection:** Tấn công hệ điều hành qua Web.
    * [ ] **IDOR / Broken Access Control:** Lỗi phân quyền.



* **Vũ khí:**
    * [ ] **Burp Suite:** Chặn và sửa gói tin (Bắt buộc phải biết).
    * [ ] **Gobuster/Dirb:** Dò tìm thư mục ẩn.

---

## 🔐 Stage 4: Crypto & Forensics (Thám tử số)
**"Giải mã bí mật và truy vết tội phạm."**

* **Cryptography (Mật mã học):**
    * [ ] Phân biệt **Encoding** (Base64, Hex) vs **Encryption**.
    * [ ] Mật mã cổ điển: Caesar, Vigenere, ROT13.
    * [ ] Mật mã hiện đại: RSA (Public/Private Key), Hashing (MD5, SHA).
    * [ ] Tool: **CyberChef**, **John the Ripper**, **Hashcat**.
* **Forensics (Điều tra số):**
    * [ ] **Steganography:** Giấu tin trong ảnh/âm thanh.
    * [ ] **Network Forensics:** Phân tích file `.pcap` tìm cờ.
    * [ ] Tool: **Steghide**, **Autopsy**, **Binwalk**, **Exiftool**.

---

## 🧠 Stage 5: Pwn & Reverse (Vùng đất thánh)
**"Nơi các pháp sư code nhị phân hội tụ."**

Đây là level khó nhất, yêu cầu hiểu sâu về kiến trúc máy tính.
* **Reverse Engineering (Dịch ngược):**
    * [ ] Đọc hiểu **Assembly** (x86/x64).
    * [ ] Phân tích file chạy (`.exe`, `.elf`) để tìm password hoặc logic ẩn.
    * [ ] Tool: **Ghidra**, **IDA Pro**, **GDB**.
* **Pwnable (Binary Exploitation):**
    * [ ] Hiểu về Stack, Heap, Memory Layout.
    * [ ] **Buffer Overflow:** Tràn bộ đệm.
    * [ ] Shellcoding: Viết mã máy để chiếm quyền điều khiển.



---

## 🚀 What's Next? (Thực chiến)

Học lý thuyết là chưa đủ. Hãy lao vào thực hành ngay:

1.  **Luyện tập (Wargames):**
    * *OverTheWire (Bandit):* Luyện Linux.
    * *PortSwigger Academy:* Luyện Web.
    * *PicoCTF:* Dành cho người mới bắt đầu (All categories).
2.  **Tham gia giải đấu:** Theo dõi `CTFtime.org` để biết lịch thi đấu.

> **Lời khuyên cuối cùng:** Đừng cố học hết tất cả cùng lúc. Hãy đi chậm, chắc, và tận hưởng cảm giác "Aha!" mỗi khi giải được một bài.
> **Chúc may mắn, Hacker!** 👨‍💻