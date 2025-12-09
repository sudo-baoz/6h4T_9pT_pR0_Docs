# 🛡️ Mission 5 — Quyền lực tối thượng (Sudo & Permissions)

> **Mục tiêu:** Trong Linux, quyền `root` là toàn năng — nhưng lạm dụng có thể phá hỏng hệ thống. Mục này giữ nguyên nội dung gốc, mở rộng giải thích, thêm ví dụ thực tế, và trình bày hiện đại hơn để bạn nắm chắc khái niệm phân quyền và cách bảo vệ hệ thống.

---

## 1. Giải mã cấu trúc quyền (`ls -l`)

Khi chạy `ls -l`, bạn sẽ thấy chuỗi như `-rwxr-xr--` — đó là "ổ khóa" của file trong Linux.

Định dạng chung: `[Loại file] [Quyền Owner] [Quyền Group] [Quyền Others]`

- **User (u)**: Chủ sở hữu file (owner).
- **Group (g)**: Nhóm người dùng liên quan.
- **Others (o)**: Tất cả người dùng còn lại.

Ba quyền cơ bản mỗi nhóm có thể có:

| Ký hiệu | Tên | Giá trị (Octal) | Mô tả ngắn |
|---:|---|:---:|---|
| `r` | Read | 4 | Đọc nội dung file/directory |
| `w` | Write | 2 | Ghi/Thay đổi/ Xóa |
| `x` | Execute | 1 | Thực thi (script/binary) hoặc truy cập thư mục |

Gộp số theo Octal để set nhanh: ví dụ `750` = `7` cho owner (`rwx`), `5` cho group (`r-x`), `0` cho others (`---`).

---

## 2. `chmod`: Thay đổi quyền (chi tiết)

`chmod` có hai cách dùng chính:

- **Numeric (Octal):** `chmod 755 file` — nhanh và phổ biến.
- **Symbolic:** `chmod u=rwx,g=rx,o= file` — rõ ràng, dễ đọc.

Tham khảo nhanh:

```bash
chmod 700 secret.sh      # Chỉ owner có quyền đầy đủ
chmod 644 public.txt      # Owner đọc/ghi, mọi người đọc được
chmod u+x script.sh       # Thêm quyền thực thi cho owner
chmod g-w file.txt        # Bỏ quyền ghi cho group
```

Mẹo: Tránh `chmod 777` — mở cửa cho tất cả, dễ dẫn đến backdoor.

---

## 3. `chown` & `chgrp`: Quản lý chủ sở hữu và nhóm

Đổi chủ sở hữu và nhóm giúp kiểm soát ai có quyền trên file.

```bash
sudo chown kali:kali malware.py   # owner = kali, group = kali
sudo chgrp admin logs/            # thay đổi group của thư mục logs
```

Kiểm tra nhanh owner/group: `ls -l file` hoặc `stat file`.

---

## 4. `sudo`: Dùng quyền tạm thời an toàn

`sudo` cho phép user thông thường tạm mượn quyền root để chạy một lệnh duy nhất.

- **Nguyên tắc:** Chỉ dùng `sudo` khi cần; tránh làm việc hàng loạt dưới `root`.
- **Kiểm tra quyền sudo của user:** `sudo -l` hiển thị lệnh bạn được phép chạy.

Thao tác hữu ích:

```bash
sudo command                # chạy 1 lệnh với quyền root
sudo -i                    # chuyển sang shell root (cẩn thận)
sudo !!                    # chạy lại lệnh trước bằng sudo
sudo -l                    # liệt kê quyền sudo của user hiện tại
```

Lưu ý bảo mật: Không chạy script không rõ nguồn dưới `sudo` nếu chưa đọc nội dung.

---

## 5. Thực hành nâng cao & tip an toàn

- **Sử dụng `umask`** để đặt mặc định quyền file khi tạo mới (ví dụ `umask 027`).
- **SetUID / SetGID / Sticky bit:** là những bit đặc biệt có thể ảnh hưởng đến hành vi file/executable. (Tìm hiểu kỹ trước khi sử dụng.)
- **Kiểm tra sudoers:** `sudo visudo` để chỉnh, luôn dùng `visudo` để tránh lỗi cú pháp.

Ví dụ kiểm tra file bằng `stat`:

```bash
stat top_secret.txt
# Hiển thị chi tiết owner/group/permissions, thời gian thay đổi
```

---

## ⚔️ Bài tập thực chiến (Lab) — giữ nguyên nội dung gốc nhưng mở rộng chỉ dẫn

Chúng ta mô phỏng tạo tài liệu mật và ngăn user khác đọc được.

**Bước 1 — Tạo user test**

```bash
sudo adduser noob_hacker
# (Nhập mật khẩu, ví dụ: 123 — chỉ dùng cho lab, KHÔNG dùng mật khẩu yếu ngoài lab)
```

**Bước 2 — Tạo tài liệu mật (ở user hiện tại, ví dụ `kali`)**

```bash
echo "Đây là bí mật quốc gia Hutech" > ~/top_secret.txt
ls -l ~/top_secret.txt
```

**Bước 3 — Khóa quyền (Hardening)**

```bash
chmod 600 ~/top_secret.txt
ls -l ~/top_secret.txt   # mong đợi: -rw-------
```

Giải thích: `600` nghĩa là owner có read+write, group/others không có quyền gì.

**Bước 4 — Kiểm thử xâm nhập (đóng vai user `noob_hacker`)**

```bash
su - noob_hacker
cat /home/kali/top_secret.txt   # sẽ báo Permission denied nếu setup đúng
exit
```

Nếu bạn muốn kiểm tra xem user `noob_hacker` có quyền sudo hay không: `sudo -l` (sẽ yêu cầu mật khẩu của `noob_hacker`).

**Bước 5 — Dọn dẹp sau lab**

```bash
exit # trở về user chính nếu còn ở noob_hacker
sudo deluser --remove-home noob_hacker
rm ~/top_secret.txt
```

---

> **Lưu ý bảo mật:** Lab dùng mật khẩu yếu chỉ để thực hành cục bộ. Trong môi trường thật, luôn dùng mật khẩu mạnh và hạn chế quyền sudo.

---

*Tôi đã giữ nguyên các bước lab gốc, bổ sung giải thích, ví dụ, và một số mẹo vận hành an toàn để bạn nắm chắc khái niệm.*