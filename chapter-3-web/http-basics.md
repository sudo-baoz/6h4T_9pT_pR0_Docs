# 🌐 Mission 13: Giải Mã Web (HTTP, Cookie & Session)

> **📜 Mission Brief:** Web không hoạt động bằng phép màu. Nó hoạt động bằng việc "Hỏi và Trả lời".
>
> Để hack được một trang web (Web Pentest), bạn không thể chỉ nhìn vào giao diện đẹp đẽ của trình duyệt. Bạn phải nhìn thấy những gì đang diễn ra ở bên dưới: Các dòng lệnh **HTTP**, những chiếc **Cookie** định danh, và cách **Burp Suite** giúp bạn thao túng tất cả.

-----

## 🏗️ MODULE 1: GIAO THỨC HTTP (NGÔN NGỮ CỦA WEB)

**HTTP** (HyperText Transfer Protocol) là giao thức phi trạng thái (Stateless). Nghĩa là Server rất "đãng trí", vừa trả lời bạn xong là nó quên bạn là ai ngay lập tức.

### 1\. Request (Yêu cầu từ Client)

Khi bạn gõ `google.com` và Enter, trình duyệt gửi đi một gói tin như sau:

```http
GET /search?q=hutech HTTP/1.1
Host: www.google.com
User-Agent: Mozilla/5.0 (Windows NT 10.0)...
Accept: text/html...
Cookie: session_id=abc123xyz
```

### 2\. Response (Phản hồi từ Server)

Server nhận lệnh, xử lý và trả về:

```http
HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8
Server: gws

<html>
  <head>...</head>
  <body>Kết quả tìm kiếm...</body>
</html>
```

### 3\. Hai phương thức "Quyền lực" (Methods)

  * **GET:** Dùng để **Lấy dữ liệu**.
      * Tham số hiện lù lù trên URL: `login.php?user=admin&pass=123`.
      * *Rủi ro:* Lộ thông tin trong lịch sử duyệt web.
  * **POST:** Dùng để **Gửi dữ liệu** (Đăng nhập, Upload ảnh).
      * Tham số ẩn trong phần Body (Thân gói tin).
      * *An toàn hơn GET*, nhưng vẫn đọc được nếu không có HTTPS.

-----

## 🍪 MODULE 2: COOKIE & SESSION (CHỨNG MINH THƯ ĐIỆN TỬ)

 Vì HTTP "đãng trí", làm sao Server biết bạn đã đăng nhập để giữ trạng thái "Online" khi bạn chuyển từ trang này sang trang khác?
 → **Cookie & Session** ra đời.

### 1\. Cookie (Lưu ở phía Khách - Browser)

Là một file text nhỏ Server cấp cho bạn sau khi đăng nhập thành công.

  * Nó giống như cái "Vòng tay tham dự sự kiện". Đi đâu bạn cũng phải giơ cái vòng (Gửi kèm Cookie) ra thì Server mới cho vào.
  * **Nguy hiểm:** Nếu Hacker trộm được Cookie của bạn (qua lỗi XSS), hắn có thể mạo danh bạn mà không cần mật khẩu (**Session Hijacking**).

### 2\. Session (Lưu ở phía Chủ - Server)

Là dữ liệu thật sự nằm trên Server.

  * Cookie chỉ chứa một cái mã định danh (Session ID, ví dụ: `PHPSESSID=xyz`).
  * Server cầm cái mã `xyz` đó, tra trong kho Session để biết: "À, đây là User Admin, đang có giỏ hàng trị giá 5 triệu".

-----

## 🛠️ MODULE 3: BURP SUITE - VŨ KHÍ TỐI THƯỢNG

Trình duyệt (Chrome/Edge) giấu hết các gói tin HTTP đi. Hacker cần một thứ đứng giữa để chặn lại và sửa đổi. Đó là **Burp Suite**.

### Tính năng cốt lõi:

1.  **Proxy (Intercept):** Chặn đứng gói tin Request trước khi nó bay tới Server. Bạn có thể sửa giá tiền từ `100$` thành `1$` rồi mới cho đi tiếp.
2.  **Repeater:** Lưu lại gói tin Request, cho phép bạn sửa đi sửa lại và gửi lại nhiều lần để test lỗi (Ví dụ: thử SQL Injection).
3.  **Intruder:** Tự động gửi hàng nghìn request để dò mật khẩu (Brute Force).

-----

## 🧪 BÀI TẬP LAB: THAO TÚNG GIÁ TIỀN (LOGIC ERROR)

**Kịch bản:** Bạn đang mua hàng trên một trang web lỗ hổng. Món hàng trị giá $1000. Bạn chỉ có $1.

**Bước 1: Thiết lập Burp Suite**

  * Mở Burp Suite → Tab **Proxy** → Bật **Intercept is On**.
  * Cấu hình trình duyệt để đi qua Proxy của Burp (hoặc dùng trình duyệt có sẵn của Burp).

**Bước 2: Mua hàng**

  * Trên web, bấm nút "Buy iPhone" (Giá 1000$).
  * Trình duyệt sẽ quay vòng vòng (đang bị Burp chặn).

**Bước 3: Chỉnh sửa gói tin**

  * Qua Burp Suite, bạn sẽ thấy gói tin Request:
    ```http
    POST /buy HTTP/1.1
    Host: shop.vuln
    ...
    product_id=10&price=1000
    ```
  * Sửa số `1000` thành `1`.

**Bước 4: Gửi đi (Forward)**

  * Bấm nút **Forward** trên Burp.
  * Kiểm tra trên Web: "Purchase Successful! -1$ deducted".
  * **Chúc mừng! Bạn vừa khai thác lỗi Business Logic.**

-----

## 🎥 VIDEO HƯỚNG DẪN (MUST WATCH)

Dưới đây là video minh họa trực quan cách Burp Suite hoạt động (bắt gói tin và chỉnh sửa).

> **📺 Video Recommendation:** Hãy lên YouTube tìm từ khóa:
> **"Burp Suite for Beginners - Intercepting and Modifying Traffic"**
> *(Kênh gợi ý: The Cyber Mentor hoặc NetworkChuck)*

-----

**Mission Completed!**
 Bạn đã hiểu cách Web vận hành.