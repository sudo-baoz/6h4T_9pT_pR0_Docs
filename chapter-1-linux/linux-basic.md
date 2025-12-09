
# 💀 Mission 4 — Kali CLI: Vũ Khí Tối Thượng (Modern)

> **Mục tiêu:** Bỏ chuột, dùng Terminal — nơi mọi cuộc tấn công bắt đầu. Nâng cấp kỹ năng CLI của bạn để thao tác nhanh, chính xác và bí mật.

---

## 🗺️ Trinh sát: Điều hướng nhanh (Navigation)

Biết mình đang ở đâu là nền tảng. Dưới đây là bảng tóm tắt các lệnh cơ bản:

| Lệnh | Mô tả | Ví dụ |
| --- | --- | --- |
| `pwd` | In đường dẫn hiện tại | `pwd` → `/usr/share/wordlists` |
| `ls -la` | Liệt kê file (bao gồm file ẩn), hiển thị quyền và kích thước | `ls -la` |
| `cd /path/to/dir` | Di chuyển giữa các thư mục | `cd /var/www/html` |

Ví dụ nhanh:

```bash
pwd                 # -> /usr/share/wordlists
ls -la              # Xem file ẩn, quyền, kích thước
cd /var/www/html    # Chuyển vào thư mục web server
```

Tip: Thư mục `/usr/share/wordlists` chứa nhiều wordlist hữu dụng (ví dụ `rockyou.txt`).

---

## 🛠️ Quản lý file & script (File Manipulation)

Tạo, xem, sao chép, di chuyển, xóa — tất cả đều từ Terminal.

- Tạo nhanh:

```bash
touch exploit.py
mkdir -p payloads
```

- Xem nội dung mà không mở editor:

```bash
cat /etc/passwd
less rockyou.txt
```

- Sao chép / di chuyển:

```bash
cp /usr/share/webshells/php/simple-backdoor.php .
mv old_name.txt new_name.txt
```

- Xóa (cẩn thận!):

```bash
rm -rf logs_folder/
```

---

## 🔐 Quyền hạn & thực thi (Permissions)

File script cần quyền thực thi để chạy. Hiểu `chmod`, `sudo`, và `root` rất quan trọng.

```bash
chmod +x install.sh   # Cấp quyền thực thi
./install.sh          # Thực thi file
```

- `sudo <command>`: Chạy lệnh dưới quyền admin.
- `sudo su` hoặc `su -`: Chuyển sang `root` (prompt đổi thành `#`).

Lưu ý: Trước khi chạy script từ internet, luôn `cat`/`less` file để kiểm tra nội dung.

---

## 🌐 Mạng & kết nối (Networking Basics)

Những lệnh cần nhớ:

```bash
ip a        # Xem địa chỉ IP (thay thế ifconfig)
ifconfig    # Còn dùng được trên nhiều bản Kali
ping 8.8.8.8
```

Xác định interface (ví dụ `eth0`, `wlan0`) để biết IP máy bạn.

---

## 🔎 Tìm kiếm & lọc (Search & Filter)

Gỡ rối, phân tích log, tìm flag — `grep` và `pipe` là bạn tốt nhất.

```bash
grep "password" access.log
history | grep ssh
cat users.txt | sort | uniq
```

Pipe (`|`) giúp kết hợp công cụ nhỏ thành chuỗi mạnh mẽ.

---

## 📦 Cài đặt công cụ (Package Management)

Kali dùng `apt`.

```bash
sudo apt update
sudo apt install gobuster
```

---

## ⚡ Phím tắt thiết yếu (Cheat Sheet)

- **`TAB`**: Tự động hoàn thành.
- **`Ctrl + C`**: Dừng tiến trình.
- **`Ctrl + L`**: Clear màn hình.
- **`↑`**: Lấy lại lệnh trước.
- **`Ctrl + Shift + C/V`**: Copy/Paste trong terminal.

---

### 🚀 Bài tập thực hành

1. Mở terminal Kali.
2. `cd /tmp`
3. `mkdir HackerLab`
4. `echo "Hutech Cyber Security" > HackerLab/secret.txt`
5. `cat HackerLab/secret.txt | grep Cyber`
6. `rm -rf HackerLab`

---

> **🎯 Nhiệm vụ tiếp theo:** Mission 5: Quyền lực tối thượng (Sudo & Permissions)

---
*Ghi chú:* Giữ thói quen kiểm tra mọi script và chạy với quyền thấp nhất cần thiết. Hãy thực hành thường xuyên để phản xạ với CLI trở nên tự nhiên.
