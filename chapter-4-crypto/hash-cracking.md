# 🔨 Mission 22: Cracking Password (John & Hashcat)

> **📜 Mission Brief:** Mật khẩu của bạn không được lưu dưới dạng văn bản (Cleartext) trên server (trừ khi server đó quá tệ). Chúng được lưu dưới dạng **Hash** (Băm) - một chuỗi ký tự ngẫu nhiên không thể dịch ngược.
>
> Nhiệm vụ của bạn: Từ chuỗi Hash vô nghĩa đó, tìm ra mật khẩu gốc để đăng nhập.
>
>   * **John the Ripper:** "Con dao đa năng", chạy tốt trên CPU, tự động nhận diện lỗi.
>   * **Hashcat:** "Quái vật tốc độ", dùng GPU (Card màn hình) để thử hàng tỷ mật khẩu mỗi giây.

-----

## 🛑 KHU VỰC CẤM: ĐẠO ĐỨC & PHÁP LÝ

> **⚠️ CẢNH BÁO:**
> Việc bẻ khóa mật khẩu của người khác (Facebook, Gmail, Wifi hàng xóm) là **HÀNH VI PHẠM PHÁP**.
>
>   * Bạn chỉ được phép crack:
>     1.  Hash trong các bài Lab/CTF.
>     2.  Mật khẩu của chính bạn (để kiểm tra độ mạnh).
>     3.  Hệ thống bạn được thuê để Pentest (có văn bản).

-----

## 🏗️ MODULE 1: HASH LÀ GÌ? (KHÔNG PHẢI MÃ HÓA)

Hacker mới thường nhầm lẫn **Encryption** (Mã hóa) và **Hashing** (Băm).

  * **Mã hóa (Encryption):** 2 chiều. Có chìa khóa là mở được. (VD: File Zip).
  * **Băm (Hashing):** 1 chiều. Giống như xay thịt. Bạn không thể biến thịt xay quay lại thành con bò.
      * Ví dụ: `123456` → MD5 Hash: `e10adc3949ba59abbe56e057f20f883e`.

**Vậy Crack kiểu gì?**
Hacker không dịch ngược. Hacker làm theo kiểu **"Thử và Sai"**:

1.  Lấy một từ trong từ điển (ví dụ: "hello").
2.  Băm nó ra → `5d4140...`
3.  So sánh với Hash mục tiêu.
      * Giống nhau → Mật khẩu là "hello".
      * Khác nhau → Thử từ tiếp theo.

-----

## 🐢 MODULE 2: JOHN THE RIPPER (THE CLASSIC)

John (JtR) là công cụ huyền thoại, có sẵn trên mọi bản Kali Linux. Nó rất thông minh, tự đoán loại Hash.

### Kịch bản: Bẻ khóa mật khẩu Linux (`/etc/shadow`)

Trong Linux, file `/etc/shadow` chứa hash mật khẩu của người dùng.

**Bước 1: Gộp file (Unshadow)**
Cần gộp file chứa user (`passwd`) và file chứa hash (`shadow`) thành một để John đọc.

```bash
unshadow /etc/passwd /etc/shadow > my_hashes.txt
```

**Bước 2: Tấn công bằng Wordlist**
Sử dụng bộ từ điển huyền thoại `rockyou.txt` (nằm ở `/usr/share/wordlists/`).

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt my_hashes.txt
```

**Bước 3: Xem kết quả**
Nếu tìm thấy, John sẽ hiện ra ngay. Hoặc xem lại bằng lệnh:

```bash
john --show my_hashes.txt
```

-----

## 🐆 MODULE 3: HASHCAT (THE BEAST)

Nếu John là xe đạp địa hình, thì Hashcat là siêu xe Ferrari. Hashcat sử dụng sức mạnh tính toán song song của Card đồ họa (GPU) để bẻ khóa cực nhanh.

### Cú pháp lệnh cơ bản:

```bash
hashcat -m [Loại_Hash] -a [Kiểu_Tấn_CÔng] [File_Hash] [Wordlist]
```

  * **`-m` (Mode):** Mã số của loại Hash.
      * `0`: MD5
      * `1000`: NTLM (Windows)
      * `1800`: SHA-512 (Linux)
      * `2500`: WPA2 (Wifi)
      * *(Tra cứu mã tại: `hashcat --help`)*
  * **`-a` (Attack Mode):**
      * `0`: Wordlist (Dùng từ điển).
      * `3`: Brute-force (Thử a-z, 0-9 toàn bộ).

### Ví dụ thực chiến: Crack MD5

Bạn có một file `hash.txt` chứa: `5f4dcc3b5aa765d61d8327deb882cf99` (Đây là MD5).

```bash
hashcat -m 0 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
```

*Giải thích:* Dùng chế độ MD5 (`-m 0`), tấn công từ điển (`-a 0`), dùng file `rockyou.txt` để đối chiếu.

-----

## ⚔️ MODULE 4: CÁC KIỂU TẤN CÔNG (ATTACK MODES)

### 1\. Dictionary Attack (Tấn công từ điển) - *Nhanh nhất*

Dùng một danh sách các từ phổ biến (password, 123456, iloveyou...).

  * **Ưu điểm:** Cực nhanh (vài giây).
  * **Nhược điểm:** Nếu mật khẩu là `Hutech@2025!#`, từ điển sẽ bó tay.

### 2\. Brute-Force Attack (Vét cạn) - *Chậm nhất*

Thử từng ký tự một: `aaaaa` → `aaaab` → `aaaac`.

  * **Ưu điểm:** Chắc chắn tìm ra (nếu đủ thời gian).
  * **Nhược điểm:** Với mật khẩu dài 8 ký tự gồm chữ hoa, thường, số, ký tự đặc biệt... bạn sẽ mất **vài thế kỷ** để tìm ra.

### 3\. Rule-based Attack (Thông minh)

Hacker kết hợp Từ điển + Quy tắc biến đổi.

  * Từ điển có chữ `password`.
  * Rule sẽ tự động thử thêm: `password123`, `Password!`, `p@ssword`.
  * Đây là cách hiệu quả nhất để bẻ các mật khẩu phức tạp vừa phải.

-----

## 🧪 BÀI TẬP LAB: GIẢI CỨU MẬT KHẨU

**Nhiệm vụ:** Tìm mật khẩu gốc của mã Hash MD5 sau: `e10adc3949ba59abbe56e057f20f883e`.

**Bước 1:** Tạo file chứa hash.

```bash
echo "e10adc3949ba59abbe56e057f20f883e" > crack_me.txt
```

**Bước 2:** Dùng Hashcat (hoặc John).
Nếu bạn dùng máy ảo không có GPU mạnh, hãy dùng **John** cho nhẹ.

```bash
john --format=Raw-MD5 crack_me.txt
```

*(Nếu dùng Hashcat: `hashcat -m 0 -a 3 crack_me.txt`)*

**Bước 3:** Chờ đợi kết quả.
Nó sẽ hiện ra: `123456` (Đây là mật khẩu phổ biến nhất thế giới).

-----

## 🛡️ MODULE 5: PHÒNG THỦ (BLUE TEAM)

Làm sao để mật khẩu không bị Crack?

1.  **Độ dài là vua:** Mật khẩu 12 ký tự trở lên cực kỳ khó crack bằng Brute-force.
2.  **Sử dụng Salt (Muối):** Thêm một chuỗi ngẫu nhiên vào mật khẩu trước khi băm.
      * Pass: `123456` → Hash giống nhau.
      * Pass: `123456` + Salt `Xi8@` → Hash khác hẳn.
      * Điều này chống lại được **Rainbow Table** (Bảng tra cứu Hash tính sẵn).
3.  **Không dùng từ có nghĩa:** Đừng dùng tên, ngày sinh, tên thú cưng. Hãy dùng câu (Passphrase): `ToiDiHocO_Hutech_VaoMuaThu`.

-----

**Mission Completed\!**
Bạn đã hiểu sức mạnh của phần cứng trong bảo mật.
