# 🏋️‍♂️ Đấu trường 2: TryHackMe - Phòng Tập Gym Cyber

> **📜 Brief:** Sau khi làm quen với các khái niệm rời rạc, bạn cần một lộ trình bài bản, kết hợp giữa lý thuyết và thực hành.
>
> **TryHackMe (THM)** được mệnh danh là "Phòng Gym" vì nó chia kiến thức thành các bài tập nhỏ, có hướng dẫn chi tiết từng bước. Bạn không cần cài máy ảo, không cần setup phức tạp, chỉ cần mở trình duyệt lên và "luyện cơ bắp".



---

## 💡 1. Tại Sao Chọn TryHackMe?

Khác với các giải CTF truyền thống (thường ném cho bạn cái đề rồi mặc kệ bạn làm sao thì làm), TryHackMe có phong cách giáo dục hơn:

1.  **Vừa học vừa hành:** Màn hình chia đôi. Bên trái là lý thuyết hướng dẫn, bên phải là máy ảo để bạn gõ lệnh thực hành ngay lập tức.
2.  **AttackBox:** THM cung cấp sẵn một máy Kali Linux trên trình duyệt (Cloud). Bạn không cần máy tính cấu hình mạnh vẫn học được.
3.  **Learning Paths (Lộ trình):** Các bài học được sắp xếp theo thứ tự từ Zero đến Hero, bạn không sợ bị "hổng kiến thức".

---

## 🗺️ 2. Lộ Trình Luyện Tập (The Roadmap)

Đừng nhảy vào giải các bài khó ngay. Hãy đi theo thứ tự sau để xây dựng nền móng vững chắc.

### Giai đoạn 1: Pre-Security (Nhập môn)
* **Mục tiêu:** Hiểu máy tính hoạt động thế nào trước khi học cách tấn công nó.
* **Nội dung:** Mạng căn bản (OSI, IP), Linux cơ bản, Web hoạt động ra sao.
* **Dành cho:** Người chưa biết gì về IT.

### Giai đoạn 2: Complete Beginner (Tân thủ) 🌟 *Quan trọng nhất*
* **Mục tiêu:** Sử dụng thành thạo các công cụ Hacker cơ bản.
* **Nội dung:** Nmap, Burp Suite, Metasploit, Hydra, Linux Privilege Escalation.
* **Kết quả:** Sau khi xong path này, bạn chính thức thoát kiếp "Newbie".

### Giai đoạn 3: Jr. Penetration Tester (Tập sự)
* **Mục tiêu:** Học các kỹ thuật tấn công chuyên sâu.
* **Nội dung:** Web Hacking (OWASP Top 10), Network Security, Active Directory.



---

## 🥗 3. Thực Đơn Cho Người Mới (Free Rooms)

Dưới đây là danh sách các phòng (Rooms) **miễn phí** và **chất lượng nhất** mà bạn nên làm ngay hôm nay:

### 1. Welcome & Basics
* **Tutorial:** Hướng dẫn cách sử dụng giao diện THM và bật máy ảo AttackBox.
* **OpenVPN:** Hướng dẫn cách kết nối máy Kali cá nhân của bạn vào mạng lưới THM (nếu không muốn dùng AttackBox).

### 2. Linux & Tools
* **Linux Fundamentals 1, 2, 3:** Series bài học dạy Linux cực hay, từ lệnh `ls` đến phân quyền và quản lý tiến trình.
* **Nmap:** Hướng dẫn quét mạng chi tiết (như Mission 10 của chúng ta nhưng có bài tập thực tế).

### 3. Web Hacking
* **OWASP Top 10:** Phòng cực dài và chi tiết, dạy bạn từng lỗ hổng một (SQLi, XSS, RCE...) kèm ví dụ thực hành.
* **Upload Vulnerabilities:** Học cách upload shell để chiếm quyền server.

### 4. CTF Challenge (Thử thách tổng hợp)
Sau khi học xong công cụ, hãy thử sức với các bài CTF hoàn chỉnh (Tìm user flag và root flag):

* **Pickle Rick:** (Dựa trên phim Rick & Morty). Bài CTF kinh điển nhất cho người mới. Không cần công cụ phức tạp, chỉ cần tư duy Web và Linux cơ bản.
* **RootMe:** Bài tập về lỗ hổng Upload file và leo thang đặc quyền cơ bản.
* **Simple CTF:** Kết hợp Nmap, lỗ hổng dịch vụ và bẻ khóa mật khẩu.

---

## 🏆 4. Bí Kíp "Cày Cuốc" Hiệu Quả

1.  **Giữ Streak (Chuỗi ngày):** THM có hệ thống tính điểm chuyên cần. Cố gắng mỗi ngày giải ít nhất 1 câu hỏi để duy trì thói quen.
2.  **Đọc Write-up khi bí:** Nếu kẹt quá 30 phút, hãy tìm google "TryHackMe [Tên Room] writeup". Đọc để hiểu tư duy của họ, sau đó tắt đi và tự làm lại.
3.  **Ghi chú (Note-taking):** Hãy dùng Notion, Obsidian hoặc CherryTree để ghi lại những lệnh mới học được. Trí nhớ của bạn sẽ phản bội bạn, nhưng ghi chú thì không.

---

**🚀 Sẵn sàng chưa?**
Truy cập [tryhackme.com](https://tryhackme.com/), tạo tài khoản và bắt đầu với **"Pre-Security"** hoặc **"Linux Fundamentals"** ngay bây giờ!