# 🌐 Mission 9: IP, Port & Protocol (Những điều phải biết)

> **Thông điệp:** Nếu Hacker là một thầy phù thủy, thì kiến thức về Mạng (Networking) chính là quyển sách ma pháp. Bạn không thể tấn công thứ mà bạn không hiểu cách nó vận hành.
>
> Chúng ta sẽ không học lý thuyết suông. Chúng ta sẽ học cách **gói tin (packet)** di chuyển, cách nó bị chặn lại bởi Firewall, và cách nó len lỏi vào máy chủ.

-----

## 🏗️ 1. Mô hình TCP/IP & Sự Đóng Gói (Encapsulation)

Internet không gửi nguyên một file ảnh 1GB đi một lúc. Nó cắt nhỏ file đó ra, đóng gói vào các lớp bảo vệ rồi mới gửi đi. Quá trình này gọi là **Encapsulation**.

![Network diagram](https://encrypted-tbn1.gstatic.com/licensed-image?q=tbn:ANd9GcQi3-y8fdiGUaFOKmpefDpx4M3LWQ1a9cpLddku177zu6bZoWsERICcsdLVo83CYolLKzD71VVy9bOVsfIW80ckKzcmglmGys2PevPeBRcn8hPF_4s)

Hãy tưởng tượng mô hình TCP/IP 4 lớp như quy trình gửi một món hàng:

1.  **Application Layer (Lớp Ứng dụng):** Món hàng (Dữ liệu/Data).
      * Giao thức: HTTP, SSH, FTP.
      * *Hacker làm gì ở đây?* Tấn công SQL Injection, XSS, Phishing.
2.  **Transport Layer (Lớp Giao vận):** Đóng thùng container (Thêm TCP/UDP Header).
      * Nó dán nhãn: "Gửi đến Cửa số mấy?" (Port Source/Destination).
      * *Hacker làm gì ở đây?* Quét Port, Tấn công DoS, Session Hijacking.
3.  **Internet Layer (Lớp Mạng):** Dán địa chỉ nhà (Thêm IP Header).
      * Nó dán nhãn: "Gửi đến Nhà số mấy?" (IP Source/Destination).
      * *Hacker làm gì ở đây?* IP Spoofing (Giả mạo IP), Man-in-the-Middle.
4.  **Network Access Layer (Lớp Vật lý):** Xe tải vận chuyển.
      * Nó dùng địa chỉ MAC (Media Access Control) để chuyển tin trong mạng LAN.
      * *Hacker làm gì ở đây?* ARP Spoofing, Sniffing WiFi.

-----

## 🏠 2. Hệ Thống Định Danh: IP & Subnet Mask

### A. IPv4 (Internet Protocol version 4)

Dù IPv6 đang tới, IPv4 vẫn là vua của CTF và Pentest hiện tại.
Cấu trúc: `x.x.x.x` (Ví dụ: `192.168.1.5`). Mỗi `x` là 8 bit, tổng cộng 32 bit.

#### Phân loại quan trọng (Hacker phải thuộc lòng):

  * **Public IP (IP Tĩnh/Động ngoài Internet):** Duy nhất trên toàn cầu.
      * Ví dụ: IP của server Google (`8.8.8.8`). Đây là mục tiêu tấn công từ xa.
  * **Private IP (IP Nội bộ/LAN):** Chỉ dùng trong nhà/công ty. Không thể truy cập trực tiếp từ ngoài Internet nếu không qua Router (NAT).
      * Class A: `10.0.0.0` đến `10.255.255.255` (Doanh nghiệp lớn).
      * Class B: `172.16.0.0` đến `172.31.255.255` (Công ty vừa, Docker).
      * Class C: `192.168.0.0` đến `192.168.255.255` (Wifi gia đình, Hutech labs).
  * **Loopback IP:** `127.0.0.1` (Localhost). Chính là máy của bạn.

### B. Subnet Mask & CIDR

Bạn thường thấy ký hiệu `/24` (Ví dụ: `192.168.1.0/24`). Đây là cách viết tắt của CIDR.

  * `/24` tương đương Subnet Mask `255.255.255.0`.
  * Nghĩa là: 3 số đầu (`192.168.1`) là địa chỉ mạng, số cuối cùng là địa chỉ máy.
  * **Tại sao cần biết?** Khi dùng Nmap, lệnh `nmap 192.168.1.0/24` sẽ quét toàn bộ 254 máy trong mạng đó. Nếu gõ sai thành `/8`, bạn sẽ quét cả triệu máy và bị chặn ngay lập tức\!

-----

## 🚪 3. Ports (Cổng) - "Lỗ hổng" của hệ thống

IP giúp bạn tìm đến đúng máy chủ, nhưng **Port** giúp bạn đi vào đúng dịch vụ.

  * Tổng số Port: 0 - 65535.
  * Trạng thái Port (Nmap sẽ báo cho bạn):
      * **Open:** Cổng đang mở, có chương trình đang chạy $\rightarrow$ **Mục tiêu tấn công.**
      * **Closed:** Cổng đóng, có phản hồi từ chối $\rightarrow$ Không có gì ở đây.
      * **Filtered:** Bị Firewall chặn, không có phản hồi $\rightarrow$ Khó nhằn.

### Các Port "Tử huyệt" thường gặp:

| Port | Dịch vụ | Nguy cơ bảo mật |
| :--- | :--- | :--- |
| **20/21** | FTP | Truyền file không mã hóa. Có thể bắt gói tin để lấy user/pass. |
| **22** | SSH | Quản trị từ xa. Thường bị tấn công Brute-force mật khẩu. |
| **23** | Telnet | Phiên bản cũ của SSH, không mã hóa. **Cực kỳ nguy hiểm.** |
| **53** | DNS | Phân giải tên miền. Có thể bị khai thác DNS Tunneling. |
| **80** | HTTP | Web không bảo mật. Dễ bị MITM (Man-in-the-Middle). |
| **443** | HTTPS | Web bảo mật. An toàn hơn, nhưng vẫn dính lỗ hổng ứng dụng web (SQLi). |
| **445** | SMB | Chia sẻ file Windows. Nơi sinh ra các mã độc tống tiền (WannaCry, EternalBlue). |
| **3306** | MySQL | Database. Nếu mở ra Internet, dễ bị dò mật khẩu. |
| **3389** | RDP | Remote Desktop Windows. Mục tiêu số 1 của Ransomware. |

-----

## 🗣️ 4. Giao thức Giao vận (Transport Protocols)

Đây là phần quan trọng nhất để hiểu về Nmap Scanning.

### 🔴 TCP (Transmission Control Protocol) - "Quý ông lịch lãm"

TCP đảm bảo độ tin cậy. Trước khi gửi dữ liệu, nó phải thiết lập kết nối.

[Image of TCP 3-way handshake diagram]

**Quy trình bắt tay 3 bước (3-Way Handshake):**

1.  **SYN (Synchronize):** Khách (Client) gửi cờ SYN ("Alo, mở cửa không?").
2.  **SYN-ACK (Synchronize-Acknowledge):** Chủ (Server) trả lời SYN-ACK ("Mở nha, vào đi").
3.  **ACK (Acknowledge):** Khách gửi ACK ("Ok, tôi vào đây"). $\rightarrow$ Kết nối thiết lập.

> **💀 Hacker Mindset:**
>
>   * **SYN Scan (Nmap -sS):** Hacker gửi SYN. Server trả lời SYN-ACK. Hacker... im lặng (hoặc gửi RST) và bỏ chạy. $\rightarrow$ Biết cổng mở mà không tạo kết nối chính thức $\rightarrow$ Khó bị phát hiện (Stealth Scan).

![WebSocket](https://encrypted-tbn0.gstatic.com/licensed-image?q=tbn:ANd9GcRhkmqrsiZvQGm7bGVGceQVACefBRvN7NgPCVrkQHxgsnus6EBv-RV0K2x_GLoE24rtxNp5mhMzRc4xk6s-bEk0E0g7zyEKy9V5xtVaq4FXRQXweF0)

### 🔵 UDP (User Datagram Protocol) - "Kẻ liều lĩnh"

UDP gửi dữ liệu ồ ạt mà không cần biết bên kia có nhận được không (Streaming, DNS, VoIP).

  * **Không có bắt tay.**
  * **Quét UDP rất khó:** Nếu bạn gửi tin đến 1 cổng UDP và không thấy trả lời, có thể cổng đó đang mở, hoặc gói tin đã bị rớt dọc đường.

### 🟢 ICMP (Internet Control Message Protocol)

Dùng để báo cáo lỗi và kiểm tra trạng thái mạng (Lệnh `ping`).

  * **TTL (Time To Live):** Mỗi gói tin có một "tuổi thọ". Đi qua mỗi Router, tuổi thọ giảm 1. Nếu về 0, gói tin bị hủy. Hacker dựa vào số TTL trả về để đoán hệ điều hành (Windows thường có TTL=128, Linux TTL=64).

-----

## 🛠️ 5. Practical Lab (Thực hành trên Kali)

Hãy mở Terminal và trở thành một Network Analyst.

### Lab 1: Nhận diện bản thân (Network Interface)

```bash
ip a
```

  * Tìm `eth0` hoặc `wlan0`.
  * Tìm dòng `inet`: Đó là IPv4 của bạn.
  * Tìm dòng `link/ether`: Đó là địa chỉ MAC của bạn.

### Lab 2: Kiểm tra định tuyến (Routing Table)

Xem máy tính của bạn đi đường nào để ra Internet.

```bash
ip route
# Dòng "default via ..." chính là địa chỉ của Router/Modem nhà bạn.
```

### Lab 3: Soi cổng mở (Netstat/SS)

Kiểm tra xem máy bạn có đang bị cài phần mềm gián điệp (Backdoor) nghe lén cổng nào không.

```bash
sudo ss -tulpn
```

  * Cột **Local Address:Port**: IP và Cổng đang mở.
  * Cột **Process**: Tên chương trình đang mở cổng đó. Nếu thấy một tên lạ hoắc (ví dụ `nc`, `python`) đang mở cổng cao (vd: 4444, 5555), coi chừng máy bạn đã bị hack\!

### Lab 4: Netcat - Con dao thụy sĩ (The Swiss Army Knife)

Mô phỏng mô hình Client-Server (Chat box đơn giản).

**Bước 1:** Mở Terminal A (Làm Server lắng nghe)

```bash
nc -lvnp 8080
# -l: Listen (Lắng nghe)
# -v: Verbose (Hiện chi tiết)
# -n: No DNS (Không phân giải tên miền cho nhanh)
# -p: Port 8080
```

*Màn hình hiện: `Listening on 0.0.0.0 8080`*

**Bước 2:** Mở Terminal B (Làm Client kết nối)

```bash
nc 127.0.0.1 8080
```

*Bây giờ gõ gì bên B thì bên A sẽ hiện và ngược lại.*

> **💡 Giải thích:** Bạn vừa tạo ra một kết nối TCP thô sơ. Các mã độc (Trojan/Backdoor) hoạt động y hệt như thế này: Hacker mở cổng trên máy nạn nhân và kết nối vào để gõ lệnh.

-----

**Nhiệm vụ hoàn thành\! Bạn đã nắm vững lý thuyết nền tảng.**
**Tiếp theo:** Bạn có muốn tôi triển khai **Mission 10: Nmap Scanning** - sử dụng kiến thức về TCP/IP vừa học để thực hiện quét dò tìm lỗ hổng thực tế không? (Phần này sẽ rất thú vị và có nhiều lệnh ngầu).