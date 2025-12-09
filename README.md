# 👋 Chào mừng đến với Kali Linux & CTF Handbook

> **"Hacking is not about breaking things. It's about understanding how things work."**
> *(Hacking không phải là phá hoại. Đó là sự thấu hiểu tường tận cách mọi thứ vận hành.)*

Chào mừng bạn đến với cuốn cẩm nang nhập môn **Kali Linux và Capture The Flag (CTF)**. Nếu bạn là sinh viên năm nhất đang loay hoay với những dòng lệnh Linux đầu tiên, một "newbie" muốn bước chân vào thế giới An toàn thông tin (ATTT), hay đơn giản là một người tò mò muốn biết *"Hacker mũ trắng thực sự làm gì trên màn hình đen ngòm đó?"* — thì chúc mừng, bạn đã tìm đúng nơi.

![Banner Kali Linux](https://cellphones.com.vn/sforum/wp-content/uploads/2021/09/00-Kali-Linux.png)
*(Đây là nơi ghi lại hành trình từ Zero đến khi giải được flag đầu tiên của chúng ta)*

---

## 🎯 Tại sao cuốn tài liệu này ra đời?

Thực tế là khi bắt đầu học An toàn thông tin, đặc biệt là tại môi trường Đại học, tôi (và có thể là cả bạn) thường gặp phải những rào cản vô hình:

1.  **Sự choáng ngợp (Overwhelmed):** Kali Linux có hơn 600 công cụ được cài sẵn. Mở Menu lên và bạn không biết bắt đầu từ đâu. *Metasploit để làm gì? Tại sao Wireshark lại nhiều nút bấm thế?*
2.  **Tài liệu "lệch pha":** Các tài liệu trên mạng thường rơi vào hai thái cực:
    * Hoặc quá sơ sài, chỉ dạy gõ lệnh mà không giải thích bản chất.
    * Hoặc quá hàn lâm (Academic), đọc 10 trang lý thuyết vẫn chưa biết cách quét một địa chỉ IP.
3.  **Khoảng cách giữa Học và Thi:** Bạn học mạng máy tính, học hệ điều hành, nhưng khi vào một giải CTF thực tế, bạn vẫn "đứng hình" trước một bài Web Exploitation đơn giản.

Cuốn giáo trình này được viết ra để **lấp đầy khoảng trống đó**. Tư duy cốt lõi ở đây là **"Thực chiến & Đơn giản hóa"**. Chúng ta sẽ không học vẹt. Chúng ta học để hiểu, để áp dụng và để chiến thắng trong các trò chơi WarGames.

---

## 🚀 Bạn sẽ nhận được gì từ hành trình này?

Tài liệu này không chỉ là một bảng danh sách các câu lệnh. Nó là một lộ trình tư duy (Mindset Roadmap) được chia thành 4 giai đoạn cụ thể:

### Giai đoạn 1: Khởi động (The Setup & Mindset)
* Cách xây dựng một phòng Lab cá nhân an toàn (Virtualization).
* Hiểu đúng về Đạo đức nghề nghiệp (White Hat vs Black Hat) và Luật An ninh mạng Việt Nam.
* Làm quen với "Hệ sinh thái" Kali Linux.

### Giai đoạn 2: Sinh tồn với dòng lệnh (Linux Survival)
* Thành thạo Terminal: Nơi bạn sẽ sống và làm việc 90% thời gian.
* Quyền hạn (Permissions), Quản lý tiến trình (Processes) và Hệ thống tệp tin (File System).
* *Mục tiêu:* Bạn có thể điều khiển máy tính mà không cần dùng chuột.

### Giai đoạn 3: Hộp công cụ của Hacker (The Toolkit)
Đi sâu vào các nhóm công cụ cho từng mảng CTF:
* **Network:** Nmap (quét cổng), Netcat (kết nối), Wireshark (phân tích gói tin).
* **Web:** Burp Suite (chặn bắt request), Gobuster (fuzzing đường dẫn).
* **Forensics & Crypto:** Các công cụ giải mã, trích xuất dữ liệu ẩn (Steganography).

### Giai đoạn 4: Thực chiến (Capture The Flag)
* Áp dụng kiến thức vào giải các bài tập mẫu từ **PicoCTF, TryHackMe, HackTheBox**.
* Cách viết Write-up (báo cáo) sau khi giải xong bài.

---

## 🛠️ Yêu cầu tiên quyết (Prerequisites)

Để học tốt giáo trình này, bạn không cần phải là thiên tài máy tính, nhưng bạn cần:
* **Một máy tính:** RAM tối thiểu 8GB (để chạy mượt máy ảo), ổ cứng trống khoảng 40GB.
* **Tiếng Anh cơ bản:** Hầu hết tài liệu gốc và thông báo lỗi (Error Log) đều là tiếng Anh. Đừng lo, tôi sẽ giải thích các thuật ngữ chuyên ngành.
* **Sự kiên nhẫn:** Đây là yếu tố quan trọng nhất. Bạn sẽ gặp lỗi. Bạn sẽ thấy màn hình đỏ lòm. Đừng bỏ cuộc, hãy học cách Google và sửa nó (Debug).
* **Kỹ năng Google:** Biết cách tìm kiếm giải pháp là 50% công việc của một kỹ sư ATTT.

---

## ⚠️ Tuyên bố miễn trừ trách nhiệm (Disclaimer)

> **⚠️ LỜI CẢNH BÁO QUAN TRỌNG:**  
> Mục đích duy nhất của tài liệu này là **GIÁO DỤC, NGHIÊN CỨU VÀ TỰ VỆ (Educational & Research Purpose Only)**.

Khi bạn nắm trong tay quyền năng khai thác hệ thống, bạn cũng đang nắm giữ trách nhiệm lớn lao. Hãy nhớ kỹ:

1.  Tuyệt đối **KHÔNG** sử dụng các kiến thức này để tấn công, xâm nhập, phá hoại hệ thống của bất kỳ cá nhân, tổ chức hay doanh nghiệp nào nếu không có sự cho phép bằng văn bản (Written Permission).
2.  Mọi hành vi xâm nhập trái phép đều vi phạm pháp luật và có thể bị truy cứu trách nhiệm hình sự.
3.  Tác giả và những người đóng góp không chịu trách nhiệm cho bất kỳ hành vi sai trái nào từ phía người đọc.

**Hãy là một White Hat Hacker: Bảo vệ hệ thống, đừng phá hủy nó.**

---

## 🤝 Kết nối & Đóng góp (Contribution)

Kiến thức là vô tận và công nghệ thay đổi từng giờ. Tài liệu này là một dự án nguồn mở (Open Source) và nó cần sự giúp sức của cộng đồng để hoàn thiện.

Nếu bạn phát hiện lỗi sai, lỗi chính tả, hoặc có một kỹ thuật mới muốn chia sẻ (Write-up hay, Tool mới), đừng ngần ngại:

* Tạo **Issue** để báo lỗi.
* Tạo **Pull Request** để đóng góp nội dung.
* Gửi email đóng góp về: `maibao123bao@gmail.com`

### Hướng dẫn đóng góp nhanh:
1.  **Fork** repository này về GitHub của bạn.
2.  Tạo branch mới: `git checkout -b feature/noi-dung-moi`.
3.  Thực hiện thay đổi (vui lòng giữ văn phong tôn trọng và mang tính giáo dục).
4.  **Push** lên và tạo Pull Request.

---

**Bạn đã sẵn sàng chưa? Hãy bật máy ảo lên, hít một hơi thật sâu và click vào chương đầu tiên ở menu bên trái!** 🚀

*Let the hacking begin!*