# 🦀 Mission 14: Burp Suite - Vũ Khí Trấn Phái (The Industry Standard)

> **📜 Mission Brief:** Trình duyệt web (Chrome, Firefox) được thiết kế để người dùng lướt web mượt mà. Nó giấu đi tất cả các chi tiết kỹ thuật.
>
> Để hack web, bạn cần một công cụ cho phép bạn **làm ngưng đọng thời gian**, chặn gói tin lại giữa đường, phẫu thuật chỉnh sửa nó rồi mới cho đi tiếp.
> **Burp Suite** chính là kẻ đứng giữa (Man-in-the-Middle) quyền lực đó.

-----

## 🛑 KHU VỰC CẤM: LUẬT BẤT THÀNH VĂN

> **⚠️ WARNING:** Burp Suite là công cụ tấn công mạnh mẽ.
>
> 1.  **Scope (Phạm vi):** Chỉ được phép chặn và sửa gói tin của các trang web thuộc sở hữu của bạn (localhost) hoặc có văn bản cho phép (Bug Bounty programs).
> 2.  **Traffic:** Khi bật Burp, **mọi** mật khẩu, thẻ tín dụng bạn nhập vào trình duyệt đều đi qua nó. **TUYỆT ĐỐI KHÔNG** dùng Burp khi đang giao dịch ngân hàng hoặc đăng nhập tài khoản cá nhân quan trọng.

-----

## 🏗️ MODULE 1: THIẾT LẬP MÔI TRƯỜNG (SETUP)

Có 2 cách dùng Burp. Cách "Hacker lười" (dùng trình duyệt có sẵn) và Cách "Hacker chuyên nghiệp" (Cấu hình chứng chỉ CA). Chúng ta sẽ đi từ dễ đến khó.

### Bước 1: Khởi động

1.  Mở Kali Linux.
2.  Tìm và mở **Burp Suite Community Edition** (Bản miễn phí).
3.  Chọn **Temporary Project** → **Next** → **Start Burp**.

### Bước 2: "Open Browser" (Cách nhanh nhất)

Burp tích hợp sẵn một trình duyệt Chromium đã được cấu hình mọi thứ (Proxy, Chứng chỉ SSL). Bạn không cần cài đặt gì thêm.

1.  Vào tab **Proxy**.
2.  Vào tab con **Intercept**.
3.  Bấm nút **Open Browser** (Màu cam).
4.  Một trình duyệt Chromium hiện lên. Hãy thử truy cập `http://testphp.vulnweb.com`.

> **💡 Lưu ý:** Nếu bạn muốn dùng Firefox/Chrome của máy chính, bạn phải cài đặt **FoxyProxy** và Import chứng chỉ **PortSwigger CA** vào trình duyệt. (Quy trình này khá dài, hãy làm quen với trình duyệt có sẵn của Burp trước).

-----

## 🕵️ MODULE 2: BỘ BA QUYỀN LỰC (CORE FEATURES)

Giao diện Burp rất rối rắm. Bạn chỉ cần quan tâm 3 tab này trong 90% thời gian làm việc.

### 1\. Proxy / Intercept (Chốt chặn thời gian)

Đây là nơi bạn chặn gói tin lại.

  * **Intercept is On:** Đang bật chế độ chặn. Bất cứ khi nào bạn bấm link hoặc nút trên trình duyệt, nó sẽ quay vòng vòng và gói tin sẽ hiện ra ở đây.
  * **Intercept is Off:** Tắt chặn. Gói tin đi qua tự do (dùng để lướt web bình thường nhưng vẫn ghi log).
  * **Nút Forward:** Cho phép gói tin hiện tại đi tiếp.
  * **Nút Drop:** Hủy gói tin này (Server sẽ không bao giờ nhận được).

### 2\. HTTP History (Nhật ký hành trình)

Nằm ngay cạnh tab Intercept.

  * Nó lưu lại **TẤT CẢ** các request/response đã đi qua Burp (kể cả khi bạn tắt Intercept).
  * Đây là nơi bạn quay lại tìm kiếm xem mình đã bỏ lỡ điều gì.

### 3\. Repeater (Phòng thí nghiệm) - *Quan trọng nhất*

Đây là nơi bạn "xào nấu" gói tin.

  * Thay vì phải quay lại trình duyệt bấm F5 liên tục để test lỗi, bạn gửi gói tin sang Repeater.
  * Tại đây, bạn sửa đổi tham số, bấm **Send**, và xem kết quả ngay lập tức bên khung Response.

-----

## 🧪 BÀI TẬP LAB: THAO TÚNG DỮ LIỆU (WALKTHROUGH)

Chúng ta sẽ thực hiện một cuộc tấn công giả lập: **Sửa đổi kết quả tìm kiếm.**

**Mục tiêu:** Trang web `http://testphp.vulnweb.com` (Trang web được phép hack).

### Bước 1: Bắt gói tin (Intercept)

1.  Trong Burp, đảm bảo **Intercept is On**.
2.  Trên trình duyệt (của Burp), vào `http://testphp.vulnweb.com`.
3.  Tìm ô Search, gõ chữ `HUTECH` và bấm **Go**.
4.  Trình duyệt sẽ treo. Quay lại Burp, bạn sẽ thấy gói tin Request đang bị kẹt lại:
    ```http
    POST /search.php HTTP/1.1
    Host: testphp.vulnweb.com
    ...
    searchFor=HUTECH&goButton=go
    ```

### Bước 2: Chuyển sang Repeater

Đừng sửa trực tiếp ở Proxy. Hãy tập thói quen chuyên nghiệp:

  * Chuột phải vào vùng code của gói tin → Chọn **Send to Repeater** (hoặc bấm phím tắt `Ctrl + R`).
  * Tab **Proxy** sẽ nhấp nháy, nhưng cứ kệ nó. Bấm **Intercept is Off** để trình duyệt chạy tiếp cho đỡ treo.

### Bước 3: Thí nghiệm tại Repeater

1.  Chuyển sang tab **Repeater**.
2.  Bạn sẽ thấy gói tin vừa bắt được nằm ở bên trái (Request).
3.  Hãy thử sửa chữ `HUTECH` thành `<h1>HACKED</h1>`.
4.  Bấm nút **Send** (Màu cam trên góc trái).

### Bước 4: Phân tích kết quả (Response)

1.  Nhìn sang khung bên phải (Response).
2.  Tìm kiếm trong đống code HTML trả về (có ô search ở dưới cùng), gõ `HACKED`.
3.  Nếu bạn thấy: `Your search result: <h1>HACKED</h1>` → Điều này chứng tỏ trang web dính lỗi **HTML Injection** hoặc **XSS** (Cross-Site Scripting). Server đã nhận code HTML của bạn và hiển thị nó mà không lọc bỏ!

-----

## 🎯 PRO TIPS: CẤU HÌNH ĐỂ KHÔNG BỊ "LOẠN" (SCOPE)

Khi lướt web, trình duyệt tải rất nhiều thứ rác (Google Analytics, Facebook tracking...). History của bạn sẽ bị ngập lụt. Hãy lọc nó đi.

1.  Vào tab **Target** → **Scope**.
2.  Bấm **Add**.
3.  Nhập tên miền mục tiêu (ví dụ: `testphp.vulnweb.com`).
4.  Quay lại tab **Proxy** → Bấm vào thanh filter (dòng chữ nhỏ phía trên danh sách history).
5.  Tích vào ô **"Show only in-scope items"**.
    → Bây giờ Burp sẽ chỉ hiện thị đúng mục tiêu của bạn, mọi thứ khác sẽ bị ẩn đi.

-----

## 🏆 NHIỆM VỤ VỀ NHÀ

1.  Cấu hình thành công Burp Suite.
2.  Truy cập `http://testphp.vulnweb.com/login.php`.
3.  Nhập User: `test`, Pass: `test`.
4.  Dùng Burp chặn gói tin Login lại và xem Username/Password được gửi đi như thế nào (POST Request).

-----

<details>
<summary>🆘 WRITE-UP: Login Interception — Ấn để mở</summary>

# 🆘 ZONE: WRITE-UP MISSION 14 (LOGIN INTERCEPTION)

> **Mục tiêu:** Bắt đứng (Intercept) gói tin đăng nhập của người dùng để xem mật khẩu được gửi đi như thế nào.
> **Đối tượng:** `http://testphp.vulnweb.com/login.php`

-----

### 🟢 BƯỚC 1: CHUẨN BỊ CHIẾN TRƯỜNG

1.  Mở **Burp Suite**.
2.  Vào tab **Proxy** → **Intercept**.
3.  Đảm bảo nút **Intercept is Off** (Tắt chặn để web load bình thường trước đã).
4.  Bấm nút **Open Browser** để mở trình duyệt của Burp.
5.  Truy cập: `http://testphp.vulnweb.com/login.php`.

-----

### 🟡 BƯỚC 2: GÀI BẪY (SETUP)

1.  Trên giao diện web, điền thông tin:
  * **Username:** `test`
  * **Password:** `test`
2.  **KHOAN BẤM LOGIN!** Hãy dừng lại ở đây.
3.  Quay lại Burp Suite, bấm vào nút **Intercept is Off** để nó chuyển thành **Intercept is On** (Bật chế độ chặn).
  * *Lúc này Burp đang phục kích, chờ bạn hành động.*

-----

### 🔴 BƯỚC 3: CẤT VÓ (CAPTURE)

1.  Quay lại trình duyệt, bấm nút **Login**.
2.  Trình duyệt sẽ "đứng hình" (Loading...).
3.  Nhìn sang Burp Suite, bạn sẽ thấy một gói tin vừa dính bẫy. Nó trông như sau:

### 🔍 BƯỚC 4: GIẢI PHẪU GÓI TIN (ANALYSIS)

Hãy nhìn vào nội dung gói tin trong tab Proxy, bạn sẽ thấy các thông tin quan trọng:

1.  **Dòng 1:** `POST /userinfo.php HTTP/1.1`
  * Đây là **POST Request**. Dữ liệu nhạy cảm không nằm trên URL.
2.  **Host:** `testphp.vulnweb.com`
  * Trang web đích đến.
3.  **Cookie:** (Có thể có hoặc chưa).
4.  **Body (Phần quan trọng nhất - Dòng cuối cùng):**
    Hãy nhìn xuống dưới cùng, sau một dòng trắng, bạn sẽ thấy:
    ```text
    uname=test&pass=test
    ```

> **💡 KẾT LUẬN:** Mặc dù trên trình duyệt, ô password hiển thị dấu chấm tròn `••••`, nhưng thực tế gói tin gửi đi qua mạng là **văn bản rõ (Cleartext)**. Nếu Hacker dùng Wireshark hoặc Burp Suite đứng giữa, họ sẽ đọc được ngay lập tức!

-----

### 🔵 BƯỚC 5: THẢ GÓI TIN (FORWARD)

Sau khi đã xem xong:

1.  Bấm nút **Forward** trên Burp để gói tin đi tiếp tới Server.
2.  Quay lại trình duyệt, bạn sẽ thấy đăng nhập thành công (hoặc thất bại tùy server xử lý).
3.  Đừng quên tắt **Intercept (Off)** khi làm xong để lướt web bình thường.

-----

### 🏆 BONUS: THAY ĐỔI SỐ PHẬN (TAMPERING)

Bạn có muốn thử một trò vui không? Làm lại quy trình trên, nhưng ở **Bước 3**:

1.  Thay vì để nguyên `uname=test`.
2.  Hãy sửa trực tiếp trong Burp thành `uname=admin`.
3.  Bấm **Forward**.

*Mặc dù bạn gõ `test` trên web, nhưng Server sẽ nhận được lệnh đăng nhập cho `admin`. Đây là cách Hacker thử đăng nhập vào tài khoản người khác mà không cần gõ lại trên giao diện!* 

</details>

-----

**Mission Completed!**
Bạn đã hoàn thành bài tập cơ bản nhất của Web Hacking.
**🚀 Lời khuyên:** Hãy luyện tập thao tác *Intercept -> Sửa -> Forward* này thật nhuần nhuyễn. Nó là thao tác bạn sẽ làm hàng nghìn lần trong tương lai