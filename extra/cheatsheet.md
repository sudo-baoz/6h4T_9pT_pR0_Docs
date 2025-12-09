# 📑 CHEATSHEET: CÁC LỆNH "KHÔNG THỂ SỐNG THIẾU"

## 1\. 📂 Tập Tin & Thư Mục (File System)

| Lệnh | Ý nghĩa | Ví dụ thực chiến |
| :--- | :--- | :--- |
| **`ls`** | Liệt kê | `ls -lah` (Liệt kê tất cả, bao gồm file ẩn, hiện dung lượng dễ đọc). |
| **`cd`** | Di chuyển | `cd -` (Quay lại thư mục vừa đứng trước đó - **Rất tiện\!**). |
| **`cp`** | Copy | `cp -r folder_goc folder_moi` (Copy cả thư mục). |
| **`mv`** | Di chuyển/Đổi tên | `mv file.txt /tmp/` hoặc `mv old.txt new.txt`. |
| **`rm`** | Xóa | `rm -rf folder/` (Xóa vĩnh viễn thư mục không hỏi han). |
| **`mkdir`** | Tạo thư mục | `mkdir -p cha/con/chau` (Tạo cả cây thư mục 1 lúc). |
| **`cat`** | Xem nhanh | `cat file.txt`. |
| **`less`** | Xem file dài | `less big_log.log` (Dùng phím mũi tên để cuộn, `q` để thoát). |
| **`head` / `tail`** | Xem đầu/đuôi | `tail -f access.log` (Theo dõi log chạy theo thời gian thực). |

-----

## 2\. 🌐 Mạng & Trinh Sát (Networking)

| Lệnh | Ý nghĩa | Ví dụ thực chiến |
| :--- | :--- | :--- |
| **`ip a`** | Xem IP | Thay thế cho `ifconfig`. Tìm dòng `inet`. |
| **`ping`** | Kiểm tra kết nối | `ping -c 4 8.8.8.8` (Chỉ ping 4 lần rồi dừng). |
| **`netstat`** | Soi cổng (Cũ) | `netstat -tulpn` (TCP, UDP, Listening, Process Name). |
| **`ss`** | Soi cổng (Mới) | `ss -tulpn` (Nhanh hơn netstat). |
| **`nmap`** | Quét cổng | `nmap -sC -sV -oN result.txt <IP>` (Quét script mặc định + version + lưu file). |
| **`curl`** | Tải/Gửi web | `curl -I <URL>` (Chỉ xem Header). |
| **`wget`** | Tải file | `wget -c <link>` (Tải tiếp nếu bị đứt mạng giữa chừng). |

-----

## 3\. 🔗 Kết Nối & Cửa Hậu (Connectivity)

| Lệnh | Ý nghĩa | Ví dụ thực chiến |
| :--- | :--- | :--- |
| **`ssh`** | Remote Server | `ssh user@ip -p 22` (Kết nối cổng 22). |
| **`ssh key`** | Dùng Key | `ssh -i id_rsa user@ip` (Đăng nhập bằng file khóa). |
| **`nc` (Client)** | Kết nối | `nc <IP> <Port>` (Chat, banner grabbing). |
| **`nc` (Server)** | Lắng nghe | `nc -lvnp 4444` (Tạo backdoor, hứng reverse shell). |
| **`nc` (File)** | Truyền file | **Nhận:** `nc -l -p 1234 > out.file`<br>**Gửi:** `nc <IP> 1234 < in.file` |

-----

## 4\. 🔎 Tìm Kiếm & Lọc (Search & Filter)

| Lệnh | Ý nghĩa | Ví dụ thực chiến |
| :--- | :--- | :--- |
| **`grep`** | Tìm chữ trong file | `grep -r "password" /var/www/` (Tìm đệ quy trong thư mục). |
| **`grep`** | Lọc kết quả | `cat log.txt \| grep -v "error"` (In ra dòng KHÔNG chứa chữ error). |
| **`find`** | Tìm file theo tên | `find . -name "*.conf"` (Tìm file cấu hình). |
| **`find`** | **Tìm file SUID** | `find / -perm -4000 2>/dev/null` (**Tuyệt chiêu CTF** để leo quyền). |
| **`locate`** | Tìm nhanh | `locate rockyou.txt` (Cần chạy `updatedb` trước). |

-----

## 5\. 🔐 Quyền Hạn & Người Dùng (Permissions)

| Lệnh | Ý nghĩa | Ví dụ thực chiến |
| :--- | :--- | :--- |
| **`chmod`** | Phân quyền | `chmod +x script.sh` (Cấp quyền chạy).<br>`chmod 600 id_rsa` (Bảo mật file khóa SSH). |
| **`chown`** | Đổi chủ | `chown user:group file` |
| **`sudo`** | Quyền Admin | `sudo !!` (Chạy lại lệnh vừa gõ với quyền sudo). |
| **`id`** | Xem user hiện tại | Kiểm tra xem mình là ai, thuộc nhóm nào. |
| **`whoami`** | Tên user | Xem mình đang đăng nhập là ai. |

-----

## 6\. ⚙️ Quản Lý Tiến Trình (Process Management)

| Lệnh | Ý nghĩa | Ví dụ thực chiến |
| :--- | :--- | :--- |
| **`ps`** | Xem tiến trình | `ps aux` (Xem toàn bộ tiến trình đang chạy). |
| **`top`** / **`htop`** | Task Manager | Xem CPU, RAM thời gian thực (`q` để thoát). |
| **`kill`** | Diệt tiến trình | `kill -9 <PID>` (Giết không tha - Force Kill). |
| **`bg`** / **`fg`** | Chạy ngầm | `Ctrl+Z` để tạm dừng, sau đó gõ `bg` để đẩy xuống chạy ngầm. |

-----

## 7\. 📦 Nén & Giải Nén (Archives)

Đây là phần dễ quên nhất\!

  * **Tar (Nén .tar.gz):** `tar -czvf ten_file.tar.gz folder/`
  * **Tar (Giải nén .tar.gz):** `tar -xzvf ten_file.tar.gz`
  * **Zip:** `zip -r file.zip folder/`
  * **Unzip:** `unzip file.zip`

-----

## ⚡ 8. Hacker's Utilities (Đồ chơi đặc biệt)

Những lệnh nhỏ nhưng cực hữu dụng trong CTF:

  * **Tạo Web Server nhanh (để tải file về máy nạn nhân):**
    ```bash
    python3 -m http.server 8000
    ```
  * **Giải mã Base64 nhanh:**
    ```bash
    echo "SGVsbG8=" | base64 -d
    ```
  * **Xem lịch sử lệnh đã gõ:**
    ```bash
    history | grep "ssh"
    ```
  * **Xóa màn hình (cho gọn):** `clear` hoặc bấm `Ctrl + L`.
  * **Thoát:** `exit` hoặc bấm `Ctrl + D`.

-----

**Mẹo cuối cùng:** Đừng cố học thuộc lòng. Hãy dùng nhiều rồi nó sẽ ngấm vào máu (Muscle Memory). Khi quên, hãy gõ `man <tên_lệnh>` (Manual) để xem hướng dẫn ngay trên Linux\!