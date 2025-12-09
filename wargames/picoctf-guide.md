# 🏁 Đấu trường 1: PicoCTF - Học Viện Hacker (Khu Tân Thủ)

> **📜 Brief:** Sau khi đã trang bị đầy đủ kiến thức từ Linux, Mạng đến Web, giờ là lúc bạn cần một nơi để "thử lửa" mà không bị ngợp.
>
> **PicoCTF** là nền tảng luyện tập an ninh mạng miễn phí lớn nhất thế giới, được phát triển bởi Đại học Carnegie Mellon (CMU). Nó được thiết kế như một trò chơi điện tử, nơi bạn giải đố để mở khóa cốt truyện. Đây là "lớp mẫu giáo" bắt buộc cho mọi Hacker Mũ Trắng.

-----

## 🎮 1. Tại Sao Lại Là PicoCTF?

Khác với các đấu trường khốc liệt như *HackTheBox* hay *CTFtime*, PicoCTF cực kỳ thân thiện:

  * **Không giới hạn thời gian:** Bạn có thể chơi bất cứ lúc nào (PicoGym).
  * **Phân loại rõ ràng:** Các bài tập chia theo Web, Crypto, Reverse, Forensics...
  * **Web Shell tích hợp:** Nếu máy bạn yếu hoặc chưa cài Kali Linux, PicoCTF cung cấp sẵn một cửa sổ Terminal ngay trên trình duyệt để bạn gõ lệnh.

-----

## 🛠️ 2. Hướng Dẫn Đăng Ký (Nhập Học)

Để bắt đầu, bạn cần một tài khoản định danh.

1.  **Truy cập:** [picoctf.org](https://picoctf.org/)
2.  **Đăng ký:** Bấm nút **Sign Up** (Góc phải).
      * Nhập **Username** (Chọn cái tên ngầu vào, nó sẽ hiện trên bảng xếp hạng).
      * Nhập **Email** và **Password**.
3.  **Xác thực:** Kiểm tra email và bấm link kích hoạt.
4.  **Vào Gym:** Sau khi đăng nhập, tìm mục **Practice** hoặc **PicoGym**. Đây là nơi chứa hàng ngàn bài tập từ các năm trước.

-----

## 🚩 3. Hướng Dẫn Giải Bài Mẫu (Walkthrough)

Chúng ta sẽ cùng nhau giải một bài thực tế để bạn hiểu quy trình "Capture The Flag" diễn ra như thế nào.

**Bài tập:** **"Obedient Cat"**

  * **Hạng mục:** General Skills (Kỹ năng chung).
  * **Độ khó:** Dễ nhất.
  * **Mục tiêu:** Ôn lại lệnh Linux cơ bản (Mission 4).

### Bước 1: Nhận nhiệm vụ

1.  Vào PicoGym, chọn bộ lọc **Category: General Skills**.
2.  Tìm bài **Obedient Cat**.
3.  Đọc đề: *"This file has a flag in plain sight (aka "in-the-clear"). Download this file..."*
4.  Bấm vào link `Download flag` để tải file về máy.

### Bước 2: Phân tích & Xử lý (Trên Kali Linux)

Giả sử file tải về nằm trong thư mục `Downloads`. Hãy mở Terminal lên:

1.  **Di chuyển tới file:**
    ```bash
    cd ~/Downloads
    ```
2.  **Kiểm tra file:**
    ```bash
    ls -l flag
    ```
    *(Bạn sẽ thấy file tên là `flag` nằm đó).*
3.  **Đọc nội dung (Dùng lệnh `cat`):**
    ```bash
    cat flag
    ```

### Bước 3: Lấy cờ (Capture Flag)

Ngay khi bạn gõ lệnh `cat`, Terminal sẽ hiện ra một chuỗi ký tự:

```text
picoCTF{s4n1ty_v3r1f13d_2fd6ed29}
```

Đây chính là **FLAG**\!

  * **Format:** Luôn bắt đầu bằng `picoCTF{...}`.
  * **Nội dung:** `s4n1ty_v3r1f13d_2fd6ed29` (Chuỗi ngẫu nhiên chứng minh bạn đã đọc được file).

### Bước 4: Ghi điểm (Submit)

  * Copy toàn bộ chuỗi `picoCTF{...}`.
  * Quay lại trang web PicoCTF.
  * Paste vào ô **Flag** bên phải bài tập và bấm **Submit Flag**.
  * **Kết quả:** Trang web báo **"Correct\!"** màu xanh lá và bạn được cộng điểm.

-----

## 💡 4. Mẹo Cho Tân Thủ (Pro Tips)

1.  **Đừng nản chí:** Nếu gặp bài khó (ví dụ: Crypto hoặc Reverse), hãy Google.
      * *Từ khóa:* "PicoCTF [Tên\_Bài\_Tập] writeup".
      * Đọc Writeup (lời giải) của người khác không phải là gian lận khi luyện tập, đó là **HỌC HỎI**. Nhưng hãy đọc để hiểu, đừng chỉ copy.
2.  **Sử dụng Web Shell:** Nếu bài yêu cầu `nc` (Netcat) mà bạn đang dùng Windows không có Kali, hãy bấm nút **Web Shell** màu xanh bên phải màn hình để dùng Linux của PicoCTF.
3.  **Học theo lộ trình:** Hãy bắt đầu với **General Skills** $\rightarrow$ **Web Exploitation** $\rightarrow$ **Cryptography** $\rightarrow$ **Forensics**. Để dành **Binary/Reverse** sau cùng.

-----

**🚀 Sẵn sàng chưa?**
Hãy vào ngay PicoCTF và giải 5 bài đầu tiên của phần **General Skills**. Đó là bài tập về nhà đầu tiên của bạn trong thế giới thực\!