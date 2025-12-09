# 🖼️ Mission 20: Steganography - Nghệ Thuật Giấu Tin Trong Ảnh

> **📜 Mission Brief:** Bạn nhận được một file ảnh `.jpg` trông rất bình thường, có thể là ảnh phong cảnh hoặc ảnh mèo. Nhưng bên trong những pixel màu sắc đó là bản kế hoạch bí mật, danh sách mật khẩu, hoặc một Flag của bài thi CTF.
>
> Nhiệm vụ của bạn: Học cách giấu dữ liệu vào ảnh và dùng công cụ "bạo lực" để bẻ khóa nó khi không có mật khẩu.

-----

## 🏗️ MODULE 1: STEGANOGRAPHY LÀ GÌ?

**Steganography** (Viết tắt: Stego) là nghệ thuật che giấu thông tin trong một phương tiện khác (ảnh, âm thanh, video) sao cho không ai nghi ngờ sự tồn tại của nó.

### Cơ chế hoạt động cơ bản (LSB - Least Significant Bit)

Một điểm ảnh (pixel) được cấu tạo bởi 3 màu: Đỏ (R), Xanh lá (G), Xanh dương (B). Mỗi màu có giá trị từ 0-255 (8 bit).

  * Ví dụ màu đỏ: `1111111**0**` (254)
  * Hacker sẽ thay đổi bit cuối cùng (bit ít quan trọng nhất - LSB) thành bit của tin nhắn bí mật.
  * Kết quả: Màu đỏ thành `1111111**1**` (255). Mắt thường không thể phân biệt được sự thay đổi màu sắc nhỏ xíu này, nhưng máy tính thì đọc được.

-----

## 🛠️ MODULE 2: CÔNG CỤ TÁC CHIẾN (THE TOOLKIT)

Chúng ta sẽ dùng 2 công cụ nổi tiếng nhất trên Kali Linux.

### 1\. Steghide - "Két sắt số"

Công cụ cổ điển dùng để giấu file text vào file ảnh (JPG, BMP) hoặc âm thanh (WAV) có đặt mật khẩu.

**Cài đặt:**

```bash
sudo apt update && sudo apt install steghide -y
```

### 2\. Stegseek - "Kẻ phá khóa"

Nếu bạn nhặt được một file ảnh bị giấu tin bằng Steghide nhưng không có mật khẩu? `Stegseek` là công cụ brute-force siêu tốc (nhanh gấp ngàn lần script thường) dùng để dò mật khẩu từ danh sách từ điển (`rockyou.txt`).

**Cài đặt (Tải file .deb từ GitHub):**

```bash
# Tải về (Check version mới nhất trên Github nếu link lỗi)
wget https://github.com/RickdeJager/stegseek/releases/download/v0.6/stegseek_0.6-1.deb

# Cài đặt
sudo apt install ./stegseek_0.6-1.deb
```

-----

## 🧪 MODULE 3: THỰC HÀNH GIẤU TIN (EMBEDDING)

**Kịch bản:** Bạn muốn giấu file `bi_mat.txt` vào trong file `hutech.jpg`.

### Bước 1: Chuẩn bị nguyên liệu

1.  Tạo tin mật:
    ```bash
    echo "HUTECH{Stego_Is_Fun_123}" > bi_mat.txt
    ```
2.  Tải một bức ảnh JPG bất kỳ trên mạng về (đặt tên là `hutech.jpg`).

### Bước 2: Tiến hành giấu tin (`embed`)

Sử dụng lệnh `steghide embed`.

```bash
steghide embed -cf hutech.jpg -ef bi_mat.txt
```

  * **`-cf` (Cover File):** File vật chủ (Ảnh gốc).
  * **`-ef` (Embed File):** File cần giấu.

> **Hệ thống sẽ hỏi mật khẩu:** Hãy nhập `iloveyou` (để lát nữa chúng ta thực hành bẻ khóa).

### Bước 3: Kiểm tra hiện trường

Bây giờ file `hutech.jpg` đã chứa bí mật.

  * Hãy mở ảnh lên xem: Nó trông vẫn y hệt ảnh gốc.
  * Kiểm tra dung lượng: Có thể nó sẽ tăng lên một chút xíu (không đáng kể).

-----

## 🕵️ MODULE 4: THỰC HÀNH TRÍCH XUẤT (EXTRACTING)

### Trường hợp 1: Chính chủ (Đã biết mật khẩu)

Nếu bạn là người nhận được ảnh và biết pass là `iloveyou`.

```bash
steghide extract -sf hutech.jpg
```

  * **`-sf` (Stego File):** File ảnh đã chứa tin mật.
  * Nhập mật khẩu: `iloveyou`.
  * **Kết quả:** File `bi_mat.txt` sẽ xuất hiện trở lại.

### Trường hợp 2: Hacker tấn công (Không biết mật khẩu)

Đây là tình huống thường gặp trong CTF. Bạn có file ảnh, bạn biết nó dùng Steghide (do đề bài gợi ý hoặc đoán), nhưng không có pass.

**Dùng Stegseek để "cạy két":**
Chúng ta sẽ dùng bộ từ điển huyền thoại `rockyou.txt` (có sẵn trong Kali).

```bash
# Cần giải nén rockyou trước nếu chưa làm
sudo gunzip /usr/share/wordlists/rockyou.txt.gz

# Tấn công
stegseek hutech.jpg /usr/share/wordlists/rockyou.txt
```

**Kết quả màn hình Stegseek:**
Nó sẽ chạy một thanh tiến trình màu xanh cực ngầu. Khi tìm thấy pass:

```text
[+] Found passphrase: "iloveyou"
[+] Original filename: "bi_mat.txt"
[+] Extracting to "hutech.jpg.out"
```

Bùm\! Password đã bị lộ và nội dung bí mật đã được trích xuất ra file `.out`.

-----

## 🔍 MODULE 5: FORENSICS CƠ BẢN KHÁC (BONUS)

Ngoài Steghide, trong ảnh còn có những chỗ giấu tin khác mà Hacker hay check.

### 1\. Exif Data (Thông tin Meta)

Mỗi bức ảnh chụp bằng điện thoại đều lưu thông tin: Tọa độ GPS, dòng máy ảnh, ngày chụp... Hacker có thể giấu Flag ở đây.

  * **Công cụ:** `exiftool`
  * **Lệnh:**
    ```bash
    exiftool hutech.jpg
    ```
    *(Hãy tìm kỹ các dòng Comment, Author, hoặc Copyright).*

### 2\. Strings (Chuỗi ký tự)

Đôi khi Flag được viết thẳng vào code nhị phân của ảnh (dạng text).

  * **Lệnh:**
    ```bash
    strings hutech.jpg | grep "HUTECH"
    # Hoặc xem 10 dòng cuối file (nơi thường giấu tin)
    strings hutech.jpg | tail -n 10
    ```

### 3\. Binwalk (Kiểm tra file lồng nhau)

Hacker có thể nhét cả một file ZIP vào trong bụng một file JPG.

  * **Lệnh:**
    ```bash
    binwalk hutech.jpg
    ```
  * Nếu thấy nó báo có file Zip/PNG bên trong, dùng `binwalk -e hutech.jpg` để trích xuất.

-----

## 🏆 BÀI TẬP VỀ NHÀ (CHALLENGE)

1.  Tạo một file ảnh Stego với mật khẩu khó (không có trong rockyou, ví dụ: `Hutech@2025!Secret`).
2.  Thử dùng Stegseek để bẻ khóa xem có được không? (Kết quả sẽ là thất bại).
3.  Điều này chứng minh: **Steganography chỉ an toàn khi Mật khẩu đủ mạnh.**

-----

**Mission Completed\!**
Bạn đã hoàn thành 20 Nhiệm vụ nền tảng xuất sắc.