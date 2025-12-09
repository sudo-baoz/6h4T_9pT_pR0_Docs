# 🔐 Mission 19: Mật Mã Học Nhập Môn (Encoding vs. Cipher)

> **📜 Mission Brief:** Dữ liệu không phải lúc nào cũng ở dạng văn bản dễ đọc (Cleartext). Hacker dùng các kỹ thuật này để giấu payload, giấu Flag, hoặc đơn giản là để máy tính truyền dữ liệu nhị phân qua văn bản.
>
> **Nhiệm vụ:** Nhận diện các dạng chuỗi ký tự lạ và biết cách biến chúng trở lại thành văn bản có nghĩa.

-----

## 🧬 MODULE 1: BASE64 (VUA CỦA CTF)

Nếu bạn thấy một chuỗi ký tự lộn xộn kết thúc bằng dấu bằng `=`, 99% đó là **Base64**.

### 1\. Đặc điểm nhận dạng

  * **Ký tự:** Gồm `A-Z`, `a-z`, `0-9`, `+`, `/`.
  * **Dấu hiệu:** Thường kết thúc bằng **`=`** hoặc **`==`** (Padding - để làm chẵn độ dài).
  * **Mục đích:** Chuyển dữ liệu nhị phân (ảnh, file zip, exe) thành dạng văn bản ASCII để truyền đi dễ dàng. Nó là **Encoding**, không phải Encryption (Ai cũng giải được).

### 2\. Ví dụ & Giải mã

  * **Ciphertext:** `SFVURUNie0hlbGxvX0NyeXB0b30=`
  * **Giải mã (Terminal):**
    ```bash
    echo "SFVURUNie0hlbGxvX0NyeXB0b30=" | base64 -d
    ```
    → **Kết quả:** `HUTECH{Hello_Crypto}`

> **💡 Mẹo:** Đôi khi bạn gặp Base32 (Chữ in hoa hết, kết thúc bằng nhiều dấu `=`) hoặc Base58 (Không có số 0, chữ O, chữ I, chữ l).

-----

## 🔢 MODULE 2: HEXADECIMAL (HỆ THẬP LỤC PHÂN)

Máy tính không hiểu chữ "A", nó hiểu số. Hex là cách biểu diễn gọn gàng của nhị phân.

### 1\. Đặc điểm nhận dạng

  * **Ký tự:** Chỉ gồm số `0-9` và chữ `A-F` (hoặc `a-f`). Không bao giờ có chữ G, H, I...
  * **Dấu hiệu:** Độ dài chuỗi thường là số chẵn.
  * **Ví dụ:** `48 65 6c 6c 6f` (Mỗi cặp 2 ký tự đại diện cho 1 byte/1 chữ cái).

### 2\. Giải mã (Terminal)

Bạn có chuỗi: `485554454348`

```bash
# Cách 1: Dùng xxd
echo "485554454348" | xxd -r -p

# Cách 2: Dùng Python (Nhanh gọn)
python3 -c "print(bytes.fromhex('485554454348').decode())"
```

→ **Kết quả:** `HUTECH`

-----

## 🏛️ MODULE 3: CAESAR CIPHER (ROTATION)

Đây là kỹ thuật cổ điển từ thời La Mã. Dịch chuyển bảng chữ cái đi một số bước nhất định.

### 1\. Nguyên lý

  * **Shift (Dịch):** Thay ký tự A bằng B (Shift 1), hoặc A bằng N (Shift 13).
  * **ROT13:** Là trường hợp phổ biến nhất. Dịch 13 ký tự. Vì bảng chữ cái có 26 chữ, nên ROT13 hai lần sẽ trở về như cũ.

### 2\. Ví dụ & Giải mã

  * **Ciphertext:** `Uryyb` (Đây là chữ Hello bị dịch 13 bước).
  * **Giải mã:**
      * **Cách thủ công:** Dùng lệnh `tr` (Translate) trong Linux.
    <!-- end list -->
    ```bash
    echo "Uryyb" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
    ```
      * **Cách hiện đại:** Dùng trang web **dcode.fr** hoặc công cụ **CyberChef** (Con dao thụy sĩ của dân Crypto).

-----

## 🐍 MODULE 4: AUTOMATION (SỬ DỤNG PYTHON)

Là một Hacker, bạn không nên phụ thuộc vào web online. Hãy viết script Python để giải mã hàng loạt.

Tạo file `decoder.py`:

```python
import base64
import codecs

# Chuỗi cần giải
secret_base64 = "U3VwZXJfSGFja2Vy"
secret_hex = "4379626572"
secret_rot13 = "Whfg_Gb_Rnfl"

print("--- DECODING REPORT ---")

# 1. Giải Base64
try:
    print(f"Base64: {base64.b64decode(secret_base64).decode()}")
except:
    print("Base64 Error")

# 2. Giải Hex
try:
    print(f"Hex:    {bytes.fromhex(secret_hex).decode()}")
except:
    print("Hex Error")

# 3. Giải ROT13 (Dùng thư viện codecs)
print(f"ROT13:  {codecs.decode(secret_rot13, 'rot_13')}")
```

-----

## 🧪 BÀI TẬP LAB: MINI CTF

Hãy mở Terminal Kali Linux và tìm ra 3 lá cờ (Flags) sau đây. Format cờ là `HUTECH{...}`.

### Challenge 1: The Messenger (Base64)

> **Đề bài:** Admin để lại tin nhắn này: `SFVURUNie0Jhc2U2NF9Jc19FYXN5fQ==`
>
> **Gợi ý:** Nhìn thấy dấu `==` ở cuối không?
> **Lệnh:** `echo "..." | base64 -d`

### Challenge 2: The Machine (Hex)

> **Đề bài:** Mật khẩu máy chủ là: `48 55 54 45 43 48 7b 48 65 78 5f 47 6f 64 7d`
>
> **Gợi ý:** Chỉ có số và chữ cái từ A-F.
> **Lệnh:** `echo "..." | xxd -r -p` (Nhớ bỏ dấu cách hoặc để nguyên tùy tool).

### Challenge 3: The Roman (Caesar/ROT13)

> **Đề bài:** Kẻ xâm nhập để lại ký tự: `UHGRPU{Ceb_Unpxre_2025}`
>
> **Gợi ý:** U dịch 13 bước thành H.
> **Lệnh:** Dùng `tr` hoặc Python script ở trên.

-----

<details>
<summary>🆘 GIẢI ĐÁP (SPOILER ALERT) — Ấn để hiển thị</summary>

*(Chỉ xem khi đã tự làm xong)*

1.  **Flag 1:** `HUTECH{Base64_Is_Easy}`
2.  **Flag 2:** `HUTECH{Hex_God}`
3.  **Flag 3:** `HUTECH{Pro_Hacker_2025}`

</details>

-----

**Mission Completed!**
Bạn đã biết cách đọc những ngôn ngữ kỳ lạ của máy tính.