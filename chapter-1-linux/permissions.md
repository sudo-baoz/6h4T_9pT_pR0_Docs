# 🛡️ Mission 5 — Quyền lực tối thượng (Sudo & Permissions)

> **Mục tiêu:** Trong Linux, quyền `root` là toàn năng — nhưng lạm dụng có thể phá hỏng hệ thống. Phần này giữ nguyên nội dung gốc nhưng được trình bày gọn, trực quan và thân thiện hơn: quick-reference, ví dụ thực tế, checklist lab và mẹo bảo mật.

---

📝 **Tóm tắt nhanh**

> - Phân quyền là nền tảng bảo mật Linux.
> - `r` = đọc, `w` = ghi, `x` = thực thi.
> - Dùng `chmod`, `chown`, `sudo` đúng cách để hạn chế rủi ro.

---

## 🔐 1 — Cách đọc `ls -l` (quick reference)

Chuỗi `-rwxr-xr--` biểu diễn quyền cho Owner / Group / Others. Dưới đây là bảng tham chiếu nhanh để bạn tra ngay:

| Ký hiệu | Quyền | Octal | Ý nghĩa ngắn |
|---|---|:---:|---|
| `r` | Read | 4 | Đọc nội dung file / liệt kê thư mục |
| `w` | Write | 2 | Ghi / sửa / xóa |
| `x` | Execute | 1 | Thực thi file hoặc vào thư mục |

Ví dụ: `750` → owner `rwx` (7), group `r-x` (5), others `---` (0).

---

## 🔧 2 — `chmod` (Numeric & Symbolic)

Chọn cách dùng tùy mục tiêu:

- Numeric (nhanh): `chmod 755 file`
- Symbolic (rõ ràng): `chmod u=rwx,g=rx,o=`

Lệnh tham khảo:

```bash
chmod 700 secret.sh    # owner có full quyền
chmod 644 public.txt   # owner rw, others r
chmod u+x script.sh    # thêm quyền thực thi cho owner
chmod g-w file.txt     # bỏ quyền ghi cho group
```

> ⚠️ Tránh `chmod 777` trừ khi bạn hiểu rõ rủi ro — nó mở cửa cho mọi tài khoản trên hệ thống.

---

## 👥 3 — `chown` & `chgrp` (quản lý owner/group)

Đổi chủ sở hữu hoặc nhóm giúp bạn kiểm soát ai thao tác được với file.

```bash
sudo chown kali:kali malware.py   # owner = kali, group = kali
sudo chgrp admin logs/            # đổi group của thư mục logs
```

Kiểm tra: `ls -l file` hoặc `stat file` để xem chi tiết.

---

## 🛠️ 4 — `sudo` (dùng quyền tạm thời)

`sudo` cho phép user chạy một lệnh với quyền root mà không cần đăng nhập root.

Thao tác hay dùng:

```bash
sudo command    # chạy 1 lệnh dưới quyền root
sudo -i         # shell root (cẩn thận)
sudo !!         # chạy lại lệnh trước với sudo
sudo -l         # liệt kê các lệnh bạn được phép chạy
```

Best practice: chỉ dùng `sudo` khi cần, đọc script trước khi chạy với quyền cao.

---

## 📌 5 — Nâng cao & Mẹo an toàn

- **`umask`**: Đặt mặc định quyền file mới (ví dụ `umask 027`).
- **SetUID / SetGID / Sticky bit**: Bit đặc biệt có thể thay đổi hành vi — tìm hiểu trước khi bật.
- **Sudoers**: Sửa bằng `sudo visudo` để tránh lỗi cú pháp gây mất quyền.

Ví dụ kiểm tra chi tiết:

```bash
stat top_secret.txt
# Hiển thị owner/group/permissions và timestamps
```

---

## ⚔️ Lab — Thực hành (Checklist)

Thực hiện từng bước và đánh dấu khi hoàn thành:

- [ ] **Tạo user test**

```bash
sudo adduser noob_hacker
# (Dùng mật khẩu đơn giản chỉ cho lab)
```

- [ ] **Tạo tài liệu mật**

```bash
echo "Đây là bí mật quốc gia Hutech" > ~/top_secret.txt
ls -l ~/top_secret.txt
```

- [ ] **Khóa quyền**

```bash
chmod 600 ~/top_secret.txt
ls -l ~/top_secret.txt   # mong đợi: -rw-------
```

- [ ] **Kiểm thử xâm nhập**

```bash
su - noob_hacker
cat /home/kali/top_secret.txt   # sẽ báo Permission denied nếu setup đúng
exit
```

- [ ] **Dọn dẹp**

```bash
exit
sudo deluser --remove-home noob_hacker
rm ~/top_secret.txt
```

---

> **Lưu ý bảo mật:** Lab sử dụng mật khẩu yếu chỉ để thực hành cục bộ. Trong môi trường thật, luôn dùng mật khẩu mạnh, MFA và giới hạn sudo.

---

_Phiên bản này được tối ưu để đọc nhanh, dễ thực hành và phù hợp cho curation trong GitBook._