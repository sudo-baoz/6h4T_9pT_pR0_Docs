# 📝 Kỹ Năng Viết Write-up: Nhật Ký Chiến Trường

> **Mục tiêu:** Biến những dòng lệnh khô khan thành một câu chuyện kỹ thuật hấp dẫn, dễ hiểu và có giá trị tham khảo.
> **Write-up là gì?** Là bài hướng dẫn chi tiết cách bạn giải quyết một vấn đề bảo mật. Nó trả lời câu hỏi: *Bạn đã làm gì? Tại sao bạn làm thế? Và kết quả là gì?*

-----

## 📸 1. Giai đoạn "Trong Trận Chiến" (Documentation)

Đừng đợi giải xong mới bắt đầu viết. Bạn sẽ quên sạch các chi tiết thú vị. Hãy ghi chép **ngay khi đang làm**.

### Quy tắc "Chụp trước, Hack sau":

1.  **Chụp ảnh màn hình (Screenshot):** Thấy một cổng lạ? Chụp. Thấy một đoạn mã nguồn khả nghi? Chụp. Thấy lỗi hiện ra? Chụp ngay lập tức\!
      * *Windows:* `Win + Shift + S`
      * *Linux:* `Shift + PrtSc` (hoặc dùng `Flameshot` - công cụ cực hay cho Linux).
2.  **Lưu lệnh (Command Logging):** Đừng chỉ nhớ mang máng. Hãy copy paste câu lệnh chính xác bạn đã dùng vào file nháp.
3.  **Ghi lại cả thất bại:** Đừng chỉ viết con đường chiến thắng. Hãy viết cả những "cái hố" (Rabbit Holes) bạn đã rơi vào. Ví dụ: *"Ban đầu tôi thử SQLi nhưng không được, sau đó tôi mới nhận ra nó là Command Injection..."*. $\rightarrow$ Điều này cho thấy tư duy giải quyết vấn đề của bạn.

-----

## 🏗️ 2. Cấu Trúc Một Write-up Chuẩn (The Blueprint)

Một Write-up chuyên nghiệp (hoặc Pentest Report) thường đi theo cấu trúc sau:

### A. Tiêu đề & Thông tin cơ bản

  * **Tên bài:** (Ví dụ: "PicoCTF - Obedient Cat")
  * **Hạng mục:** (Web, Crypto, Pwn...)
  * **Độ khó:** (Dễ/Trung bình/Khó)
  * **Điểm số:** (Nếu có)

### B. Trinh sát (Reconnaissance)

Mô tả bước đầu tiên khi bạn tiếp cận mục tiêu.

  * Bạn đã quét Nmap ra cái gì?
  * Giao diện web trông thế nào?
  * Bạn tìm thấy file ẩn nào bằng Gobuster?

### C. Phân tích & Suy luận (Analysis)

Tại sao bạn lại chọn hướng tấn công đó?

  * *"Tôi thấy URL có tham số `id=1`, tôi nghi ngờ có lỗi SQLi..."*
  * *"Tôi thấy server chạy Apache phiên bản cũ, tôi tìm CVE..."*

### D. Khai thác (Exploitation) - *Phần chính*

Hướng dẫn từng bước (Step-by-step) để tái hiện lại cuộc tấn công.

  * Cung cấp Payload cụ thể.
  * Hình ảnh minh họa kết quả (PoC - Proof of Concept).

### E. Lấy cờ & Kết luận (Flag & Remediation)

  * Show cái Flag ra (thường che đi một phần nếu bài thi còn đang diễn ra).
  * **(Quan trọng cho đi làm):** Đề xuất cách khắc phục lỗ hổng.

-----

## ✍️ 3. Ví dụ Mẫu (Sample Write-up)

Dưới đây là một bản Write-up mẫu ngắn gọn cho một bài CTF giả định.

-----

### [Write-up] CTF Zone: The Secret Login

**Category:** Web Exploitation | **Difficulty:** Easy

#### 1\. Trinh sát (Recon)

Tôi truy cập vào trang web `http://challenge.local` và thấy một form đăng nhập đơn giản.
Tôi thử view source (`Ctrl+U`) để xem có comment ẩn nào không nhưng không thấy gì đặc biệt.

Tôi thử đăng nhập với các tài khoản mặc định:

  * `admin / admin` -\> Thất bại.
  * `admin / password` -\> Thất bại.

#### 2\. Phân tích (Analysis)

Khi tôi nhập dấu nháy đơn `'` vào ô Username (`admin'`), trang web trả về lỗi:

> *Error: You have an error in your SQL syntax near ''' at line 1*

$\rightarrow$ **Nhận định:** Trang web dính lỗi **SQL Injection (Error-based)**. Database có vẻ là MySQL.

#### 3\. Khai thác (Exploitation)

Mục tiêu là đánh lừa câu lệnh SQL để đăng nhập không cần mật khẩu.
Tôi sử dụng payload kinh điển để tạo ra điều kiện "Luôn Đúng" (True).

**Payload:**

```sql
admin' OR 1=1 -- 
```

  * `'` : Đóng chuỗi tên người dùng.
  * `OR 1=1` : Điều kiện luôn đúng.
  * ` --  ` : Comment (bỏ qua phần kiểm tra mật khẩu phía sau).

Tôi nhập payload vào ô Username và bấm Login.

#### 4\. Kết quả (The Flag)

Đăng nhập thành công\! Tôi được chuyển hướng đến trang Dashboard và nhìn thấy cờ.

**Flag:** `CTF{SQL_Injection_Is_Still_Alive}`

#### 5\. Bài học (Lessons Learned)

Lập trình viên đã nối chuỗi trực tiếp từ input người dùng vào câu lệnh SQL.
**Cách khắc phục:** Sử dụng **Prepared Statements** (Câu lệnh chuẩn bị) để tách biệt dữ liệu và mã lệnh.

-----

## 🛠️ 4. Công cụ soạn thảo nên dùng

Đừng viết bằng Word. Hãy dùng **Markdown** (như bạn đang thấy). Nó hiển thị code đẹp, nhẹ và chuyên nghiệp.

1.  **Obsidian:** (Khuyên dùng) App ghi chú cực mạnh, hỗ trợ Markdown, link các bài viết với nhau. Dùng để xây dựng "Bộ não thứ hai" (Second Brain).
2.  **Github Gist / Repository:** Nơi lưu trữ Write-up online để làm Portfolio xin việc.
3.  **Notion:** Đẹp, đa năng, dễ chia sẻ, nhưng gõ code không sướng bằng 2 cái trên.
4.  **CherryTree:** Công cụ ghi chú dạng cây cổ điển có sẵn trên Kali Linux (được các Hacker thi OSCP rất chuộng).

-----

### 💡 Lời khuyên cuối cùng:

> Một Write-up tốt là một Write-up mà **bạn của 6 tháng sau** đọc lại vẫn hiểu mình đã làm cái quái gì. Hãy viết cho chính bản thân mình trong tương lai\!