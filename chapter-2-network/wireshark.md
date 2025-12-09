# 🦈 Mission 11: Wireshark - Bắt trọn từng gói tin

### (Network Forensics & Packet Analysis)

> **📜 Mission Brief:** Nếu hệ thống mạng là một cơ thể sống, thì dòng dữ liệu (Traffic) chính là máu.
>
> **Wireshark** là chiếc kính hiển vi điện tử giúp bạn nhìn thấy từng tế bào trong dòng máu đó. Không có gì ẩn giấu được trước Wireshark. Một khi gói tin (Packet) đã rời khỏi card mạng, nó là bằng chứng vĩnh viễn.

-----

## 🏗️ MODULE 1: GIẢI PHẪU GIAO DIỆN (INTERFACE ANATOMY)

Đừng để hàng ngàn dòng chữ chạy loạn xạ làm bạn hoảng sợ. Giao diện Wireshark chia làm 3 khu vực chiến lược.

### 1\. Packet List (Dòng thời gian - Trên cùng)

Đây là cái nhìn toàn cảnh. Mỗi dòng là một gói tin.

  * **No.:** Số thứ tự.
  * **Time:** Thời gian bắt được (tính bằng giây).
  * **Source / Destination:** Kẻ gửi và Kẻ nhận.
  * **Protocol:** Ngôn ngữ giao tiếp (TCP, UDP, HTTP, TLS...).
  * **Info:** Tóm tắt nội dung.

### 2\. Packet Details (Bàn mổ xẻ - Ở giữa) 🌟 *Quan trọng nhất*

Nơi bạn bóc tách từng lớp của gói tin theo mô hình OSI.

  * **Frame (Layer 1):** Dây cáp, độ dài gói tin.
  * **Ethernet II (Layer 2):** Chứa địa chỉ **MAC**.
      * *Mẹo:* Địa chỉ MAC cho biết hãng sản xuất (Apple, Dell, Vmware...). Nếu thấy MAC lạ trong mạng nội bộ &rarr; Có kẻ xâm nhập.
  * **Internet Protocol (Layer 3):** Chứa địa chỉ **IP**.
  * **Transmission Control Protocol (Layer 4):** Chứa **Port** (Cổng).
  * **Application (Layer 7):** Dữ liệu thực (HTTP, DNS...).

### 3\. Packet Bytes (Dữ liệu thô - Dưới cùng)

Hiển thị nội dung dưới dạng **Hex Dump** (Thập lục phân).

  * Bên trái: Mã máy đọc.
  * Bên phải: Mã người đọc (ASCII).
  * *CTF Tip:* Đôi khi Flag nằm lộ liễu ngay ở đây. Hãy soi kỹ cột bên phải\!

-----

## 🔍 MODULE 2: BỘ LỌC THẦN THÁNH (DISPLAY FILTERS)

Giữa biển dữ liệu, kỹ năng lọc (Filter) phân định Newbie và Pro. Thanh Filter nằm ngay trên cùng.

### 🔰 Cấp độ 1: Lọc cơ bản

| Mục tiêu | Lệnh Filter | Giải thích |
| :--- | :--- | :--- |
| **Tìm nạn nhân** | `ip.addr == 192.168.1.5` | Chỉ hiện traffic của IP này (cả gửi và nhận). |
| **Chỉ xem Web** | `http` hoặc `tcp.port == 80` | Lọc giao thức Web không bảo mật. |
| **Tìm DNS** | `dns` | Xem người dùng đang truy cập trang web nào (kể cả HTTPS). |
| **Tìm file ảnh** | `http contains ".jpg"` | Tìm gói tin có chứa đuôi file ảnh. |

### 🚀 Cấp độ 2: Lọc Logic (Hacker dùng cái này)

Sử dụng toán tử: `&&` (Và), `||` (Hoặc), `!` (Không/Loại trừ).

  * **Lọc Web traffic của 1 IP cụ thể:**
    ```text
    ip.addr == 192.168.1.5 && http
    ```
  * **Loại bỏ rác (ARP và SSDP thường rất nhiều):**
    ```text
    !(arp || ssdp)
    ```
  * **Tìm kiếm nội dung (Keyword Search):**
    Tìm xem có gói tin nào chứa chữ "password" không.
    ```text
    frame contains "password"
    ```
    *(Câu lệnh này cực mạnh trong các bài CTF\!)*

-----

## 🕵️ MODULE 3: KỸ NĂNG ĐIỀU TRA SỐ (FORENSICS)

Trong CTF, bạn thường nhận được một file `.pcap` (Packet Capture). Nhiệm vụ của bạn là tìm ra lá cờ (Flag) hoặc file bị đánh cắp.

### 1\. Follow TCP Stream (Ghép nối hội thoại)

Đọc từng gói tin rời rạc rất đau đầu. Wireshark có thể ghép chúng lại thành một đoạn chat hoàn chỉnh.

  * **Thao tác:** Chuột phải vào gói tin HTTP/TCP bất kỳ &rarr; **Follow** &rarr; **TCP Stream**.
  * **Hiển thị:**
      * **Màu Đỏ:** Client gửi đi (Username, Password, Request).
      * **Màu Xanh:** Server trả lời (Nội dung trang web, File ảnh).

### 2\. Export Objects (Trích xuất file từ mạng)

Nếu Hacker tải về một con virus `.exe` hoặc một tấm ảnh `.png` qua HTTP, bạn có thể lấy lại file gốc đó từ file pcap.

  * **Thao tác:** Vào menu **File** &rarr; **Export Objects** &rarr; **HTTP**.
  * Một danh sách hiện ra. Chọn file nghi ngờ và bấm **Save**.
  * *Kết quả:* Bạn đã phục hồi được bằng chứng\!

-----

## ⚔️ MODULE 4: NHẬN DIỆN TẤN CÔNG (ATTACK SIGNATURES)

Làm sao để biết mạng đang bị tấn công chỉ bằng cách nhìn Wireshark?

### 1\. ARP Spoofing (Đầu độc ARP - Man in the Middle)

  * **Dấu hiệu:** Bạn thấy một địa chỉ IP (ví dụ Gateway `192.168.1.1`) nhưng lại có **2 địa chỉ MAC khác nhau** liên tục xuất hiện.
  * **Cảnh báo:** Wireshark sẽ hiện dòng màu vàng: *"Duplicate IP address detected"*.

### 2\. Syn Flood (Tấn công DoS)

  * **Dấu hiệu:** Hàng ngàn gói tin màu xám/đen liên tục.
  * **Chi tiết:** Chỉ thấy cờ `SYN` (xin kết nối) gửi đi tới tấp mà không thấy `ACK` trả lời, hoặc Server trả lời `SYN/ACK` nhưng Client không phản hồi.

### 3\. Brute Force

  * **Dấu hiệu:** Hàng trăm gói tin `HTTP POST` hoặc `SSH Request` gửi đến cùng một server trong thời gian ngắn (vài giây).
  * **Phân tích:** Dùng `Follow Stream` để xem kẻ tấn công đang thử những mật khẩu nào.

-----

## 🔐 MODULE 5: ĐIỂM YẾU CỦA WIRESHARK (ENCRYPTION)

Wireshark là vua, nhưng nó không phải thần thánh.
Nếu trang web dùng **HTTPS (TLS/SSL)** (có ổ khóa 🔒), dữ liệu trong phần Packet Bytes sẽ hoàn toàn là rác (ngẫu nhiên).

**Bạn sẽ thấy gì?**

  * Protocol: `TLSv1.2` hoặc `TLSv1.3`.
  * Info: `Application Data`.
  * Nội dung: Không thể đọc được.

> **💡 Pro Tip:** Trong môi trường Lab kiểm thử, bạn có thể thiết lập biến môi trường `SSLKEYLOGFILE` trên trình duyệt để trích xuất khóa giải mã, sau đó nạp vào Wireshark để giải mã traffic HTTPS. (Kỹ thuật nâng cao).

-----

## 🧪 BÀI TẬP LAB: SNIFFING TELNET (Cổ Điển)

Hãy thực hành để thấy tại sao các giao thức cũ lại nguy hiểm.

1.  **Thiết lập:** Cài đặt dịch vụ Telnet trên máy ảo (hoặc dùng trang `telehack.com`).
2.  **Khởi động:** Bật Wireshark &rarr; Chọn card mạng (eth0/wlan0) &rarr; Start Capture.
3.  **Hành động:** Mở Terminal, gõ `telnet telehack.com`, sau đó thử đăng nhập hoặc gõ lệnh bất kỳ.
4.  **Dừng lại:** Bấm nút Đỏ trên Wireshark để dừng bắt gói.
5.  **Phân tích:**
      * Filter: `telnet`.
      * Chuột phải vào gói tin &rarr; **Follow TCP Stream**.
6.  **Kết quả:** Bạn sẽ thấy toàn bộ những gì mình vừa gõ hiện ra rõ mồn một\!

-----

**Mission 11 Completed\!**
Bạn đã sở hữu "con mắt thần".
