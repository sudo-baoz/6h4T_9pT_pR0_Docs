# ⚔️ Đấu trường 3: HackTheBox - Chiến Trường Thực (The Real Battlefield)

> **📜 Brief:** Nếu PicoCTF là trường mẫu giáo, TryHackMe là phòng tập Gym có huấn luyện viên (PT), thì **HackTheBox (HTB)** chính là đấu trường **UFC**.
>
> Tại đây, không còn ai cầm tay chỉ việc. Bạn được thả vào một mạng lưới chứa các máy chủ bị lỗi. Nhiệm vụ của bạn là tự tìm đường đột nhập, chiếm quyền và lấy cờ. Đây là tiêu chuẩn vàng để đánh giá kỹ năng của một Pentester trong mắt nhà tuyển dụng.

-----

## 💀 1. HackTheBox Là Gì?

HTB là một nền tảng mô phỏng mạng nội bộ (Lab) khổng lồ.

  * **Cơ chế:** Bạn kết nối máy tính của mình vào mạng của HTB thông qua VPN.
  * **Mục tiêu:** Có hàng trăm máy chủ (Machines) với các lỗ hổng từ dễ đến khó (Easy → Insane).
  * **Nhiệm vụ:**
    1.  **User Flag:** Xâm nhập vào máy, đọc file `user.txt` (Chứng minh hack được user thường).
    2.  **Root Flag:** Leo thang đặc quyền (Privilege Escalation), đọc file `root.txt` (Chứng minh đã chiếm trọn quyền Admin).

-----

## 🛠️ 2. Điều Kiện Tham Chiến (Requirements)

Đừng vội nhảy vào HTB nếu bạn chưa chuẩn bị kỹ. Bạn sẽ bị "ăn hành" ngập mặt và nản chí ngay lập tức.

### A. Trang bị kỹ thuật

1.  **Kali Linux / Parrot OS:** Bắt buộc. Bạn cần một kho vũ khí cài sẵn (Nmap, Metasploit, Burp Suite, Wordlists...).
2.  **Kết nối OpenVPN:** Bạn phải biết cách tải file `.ovpn` từ HTB và kết nối trên Terminal:
    ```bash
    sudo openvpn lab_connection.ovpn
    ```
3.  **Kiến thức nền tảng:** Phải hoàn thành ít nhất **Stage 3 (Web)** và **Stage 5 (Pwn/Linux)** trong lộ trình Roadmap.

### B. Tâm lý chiến (Mindset)

  * **Không có hướng dẫn:** HTB không có cột bên trái chỉ bạn gõ lệnh gì đâu. Bạn phải tự Google, tự đọc tài liệu.
  * **Sự kiên nhẫn:** Một bài Easy có thể ngốn của bạn 3-5 tiếng. Một bài Hard có thể mất cả tuần.
  * **"Try Harder":** Đây là khẩu hiệu của họ. Đừng bỏ cuộc khi Nmap không ra gì.

-----

## 🗺️ 3. Quy Trình Tấn Công Chuẩn (The Lifecycle)

Khi đối mặt với một "Cái hộp" (Box) trên HTB, quy trình luôn là:

1.  **Enumeration (Trinh sát):**
      * Dùng `nmap` quét cổng.
      * Dùng `gobuster` quét web.
      * Dùng `smbclient` soi file share.
2.  **Foothold (Đặt chân):**
      * Tìm lỗ hổng (CVE, Weak Password, Upload Shell).
      * Tạo Reverse Shell để chui vào máy nạn nhân. → **Lấy User Flag**.
3.  **Privilege Escalation (Leo thang):**
      * Dùng `LinPEAS` (Linux) hoặc `WinPEAS` (Windows) để tìm lỗi cấu hình.
      * Khai thác Kernel, SUID, Cronjob... để lên quyền Root/System. → **Lấy Root Flag**.

-----

## 🚀 4. Chiến Thuật Cho Người Mới (Starting Point)

Đừng lao đầu vào các máy **Active** (Máy đang hoạt động, cấm xem lời giải) ngay. Hãy bắt đầu từ khu vực dành riêng cho người mới.

### Bước 1: Khu vực "Starting Point"

HTB đã tạo ra một lộ trình tên là **Starting Point** (Tier 0, 1, 2).

  * Đây là những máy cực dễ, có file PDF hướng dẫn từng bước (Walkthrough).
  * **Mục tiêu:** Giúp bạn làm quen với việc kết nối VPN và quy trình hack cơ bản.
  * *Bắt buộc phải hoàn thành Tier 0 và Tier 1 trước khi đi tiếp.*

### Bước 2: Máy "Retired" (Đã về hưu)

Sau khi xong Starting Point, hãy chơi các máy **Retired**.

  * Đây là các máy cũ.
  * **Lợi thế:** Bạn được phép xem Write-up (Lời giải) của người khác.
  * **Cách học:** Hãy thử tự làm 30 phút. Nếu bí, mở Write-up ra đọc đúng đoạn mình bí, rồi tắt đi làm tiếp. Đừng copy paste mù quáng\!

### Bước 3: Máy "Active" (Thực chiến)

Khi đã tự tin, hãy tấn công các máy Active màu xanh lá (Easy).

  * Nếu giải được máy này, bạn sẽ được cộng điểm và thăng hạng (Rank) trên bảng xếp hạng toàn cầu.
  * Tuyệt đối không được hỏi xin Flag hay spoil lời giải.

-----

## 🏆 5. Lời Khuyên Cuối Cùng

1.  **Ghi chép (Note-taking):** Hãy dùng **CherryTree**, **Obsidian** hoặc **Notion**. Ghi lại mọi cổng mở, mọi phiên bản phần mềm bạn tìm thấy. Rất nhiều khi bạn bí là do bạn quên mất một chi tiết nhỏ đã quét được từ đầu.
2.  **VIP Subscription:** Nếu có điều kiện (khoảng 14$/tháng), hãy mua gói VIP.
      * Nó cho phép bạn truy cập toàn bộ máy Retired (kho tàng kiến thức vô giá).
      * Server VIP chạy mượt hơn và ít bị người chơi khác phá đám (Reset máy).
3.  **Cộng đồng:** Tham gia Discord của HackTheBox. Mọi người không cho bạn Flag, nhưng họ sẽ cho bạn "Gợi ý" (Nudge) khi bạn đi vào ngõ cụt.

-----

**🔥 Sẵn sàng chưa?**
Hãy tải file VPN, bật Kali Linux lên và ping thử máy đầu tiên trong **Starting Point**.
Chúc may mắn trên chiến trường thực sự\! **Happy Hacking\!**