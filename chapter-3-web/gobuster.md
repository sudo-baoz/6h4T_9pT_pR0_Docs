# 🕵️‍♂️ Mission 15: Gobuster - Tìm Ẩn Số (Directory Brute-force)

> **📜 Mission Brief:** Website giống như một ngôi nhà có nhiều phòng. Chủ nhà chỉ treo biển dẫn bạn vào phòng khách. Nhưng Hacker muốn tìm "phòng ngủ", "kho báu", hoặc "lối đi bí mật".
>
> **Gobuster** là công cụ giúp bạn gõ cửa từng phòng một cách siêu tốc để xem phòng nào có người, phòng nào bị khóa.

-----

## 🏗️ MODULE 1: NGUYÊN LÝ HOẠT ĐỘNG (THE GUESSING GAME)

Gobuster hoạt động dựa trên cơ chế **Brute-force (Dò đoán)**.
Bạn cung cấp cho nó một **Wordlist** (Danh sách từ điển chứa hàng ngàn cái tên phổ biến như: `admin`, `login`, `backup`, `config`...).

Gobuster sẽ tự động ghép tên đó vào sau địa chỉ web và gửi yêu cầu:

1.  `example.com/admin` → Server trả lời: **404 Not Found** (Không có).
2.  `example.com/login` → Server trả lời: **200 OK** (Có hàng!).
3.  `example.com/backup` → Server trả lời: **403 Forbidden** (Có, nhưng cấm vào).

-----

## 🛠️ MODULE 2: CÀI ĐẶT & CHUẨN BỊ

### 1\. Cài đặt

Trên Kali Linux, Gobuster thường có sẵn. Nếu chưa, cài cực nhanh:

```bash
sudo apt update && sudo apt install gobuster -y
```

### 2\. Vũ khí quan trọng nhất: Wordlist

Gobuster chỉ giỏi khi Wordlist của bạn xịn. Trong Kali, kho từ điển nằm ở `/usr/share/wordlists`.

  * **Bộ cơ bản (Khuyên dùng):** `/usr/share/wordlists/dirb/common.txt`
  * **Bộ nâng cao:** `/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`

-----

## ⚙️ MODULE 3: CÂU LỆNH CHIẾN ĐẤU (COMMAND ANATOMY)

Cấu trúc lệnh chuẩn để quét thư mục (`dir` mode):

```bash
gobuster dir -u <URL_Mục_tiêu> -w <Đường_dẫn_Wordlist>
```

### Giải phẫu tham số:

| Tham số | Ý nghĩa | Ví dụ |
| :--- | :--- | :--- |
| **`dir`** | Chế độ quét thư mục/file (Directory). | Luôn đặt đầu tiên. |
| **`-u`** | **U**RL mục tiêu. | `https://example.com` |
| **`-w`** | **W**ordlist (Từ điển). | `/usr/share/wordlists/dirb/common.txt` |
| **`-t`** | **T**hreads (Số luồng). | `-t 50` (Mặc định là 10. Tăng lên để quét nhanh hơn, nhưng dễ bị chặn). |
| **`-x`** | E**x**tensions (Đuôi file). | `-x php,html,txt` (Tìm thêm file cụ thể thay vì chỉ thư mục). |

-----

## 📊 MODULE 4: ĐỌC VỊ KẾT QUẢ (STATUS CODES)

Khi Gobuster chạy, màn hình sẽ chạy như ma trận. Bạn cần nhìn vào cột **Status** để biết cái nào giá trị.

  * **200 (OK):** ✅ **KHO BÁU!** Trang này truy cập được và nội dung hiện ra bình thường. (Ví dụ: `/login.php`).
  * **301 / 302 (Redirect):** ↪️ **Chuyển hướng.** Thường dẫn đến trang đăng nhập.
  * **403 (Forbidden):** ⛔ **Bị cấm.** Server xác nhận thư mục này CÓ TỒN TẠI, nhưng bạn không có quyền xem.
      * *Hacker nghĩ gì?* "Chỗ này chắc chắn giấu cái gì quan trọng (file config, .htaccess). Thử tìm cách bypass xem."
  * **404 (Not Found):** 🗑️ **Rác.** Không tồn tại, bỏ qua.

-----

## 🧪 BÀI TẬP LAB: SĂN TÌM KHO BÁU

Chúng ta sẽ thực hành quét trang web test của Acunetix.

**Bước 1: Chuẩn bị lệnh**

```bash
gobuster dir -u http://testphp.vulnweb.com -w /usr/share/wordlists/dirb/common.txt -t 50
```

**Bước 2: Quan sát kết quả**
Màn hình sẽ hiện ra:

```text
/admin                (Status: 301)
/images               (Status: 301)
/CVS                  (Status: 200)
/login.php            (Status: 200)
/userinfo.php         (Status: 200)
```

**Bước 3: Phân tích chiến lợi phẩm**

  * `/admin`: Ồ, có trang quản trị\! Thử truy cập `http://testphp.vulnweb.com/admin` xem sao.
  * `/userinfo.php`: Nghe có vẻ chứa thông tin nhạy cảm.
  * `/images`: Thư mục chứa ảnh, không quan trọng lắm.

-----

## 🚀 PRO TIP: TÌM FILE SAO LƯU (BACKUP FILES)

Admin thường hay lười và để lại các file backup ngay trên server. Đây là lỗi sơ đẳng nhưng cực kỳ nguy hiểm.
Hãy thêm cờ `-x` để tìm chúng.

```bash
gobuster dir -u http://target.com -w common.txt -x zip,bak,sql,tar.gz
```

  * Nếu bạn tìm thấy `db_backup.sql` hoặc `source_code.zip` → **GAME OVER** cho Admin đó. Bạn có toàn bộ dữ liệu!

-----

## ⚠️ CẢNH BÁO: ĐỪNG QUÁ ỒN ÀO

Gobuster gửi hàng nghìn request mỗi phút.

1.  **Server yếu:** Có thể bị sập (DoS).
2.  **Server mạnh:** Tường lửa (WAF) sẽ phát hiện IP của bạn đang spam request và chặn ngay lập tức (Block IP).
      * *Giải pháp:* Giảm tốc độ (`-t 10`) hoặc thêm thời gian nghỉ (`--delay`).

-----

**Mission Completed!**
Bạn đã có bản đồ đầy đủ của mục tiêu.