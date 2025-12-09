# Chương 1: Xây dựng Lab với VMware & Kali ISO

> **"Không có con đường tắt nào dẫn đến thành công. Việc tự tay cài đặt hệ điều hành là bài học đầu tiên về sự kiên nhẫn."**

Trong chương này, chúng ta sẽ xây dựng **Phòng thí nghiệm (Lab)** bằng cách cài đặt "tươi" Kali Linux từ file ISO gốc. Việc này giống như bạn mua linh kiện về tự ráp PC thay vì mua máy bộ vậy – bạn sẽ kiểm soát được mọi thứ.

<p align="center">
    <img src="https://img.shields.io/badge/Level-Beginner-brightgreen?style=for-the-badge" />
    <img src="https://img.shields.io/badge/Topic-Lab%20Setup-blue?style=for-the-badge" />
</p>

<p align="center"><strong>💻⚡ GenZ Hacker Vibes — nhanh, trực quan, thực chiến</strong></p>

## 1. Chuẩn bị nguyên liệu

Trước khi nấu ăn, chúng ta cần đi chợ. Bạn cần tải 2 thứ sau:

### A. VMware Workstation Player
Đây là phần mềm giả lập máy tính.
* Tải tại: [Trang chủ VMware](https://www.vmware.com/products/workstation-player.html)
* Cài đặt như bình thường (Next > Next > Finish).

### B. Kali Linux Installer (ISO)
Đây là bộ cài đặt hệ điều hành.
1. Truy cập: [https://www.kali.org/get-kali/#kali-installer-images](https://www.kali.org/get-kali/#kali-installer-images)
2. Chọn mục **"Installer Images"**.
3. Bấm nút Download ở bản **Recommended** (thường là 64-bit). File nặng khoảng 3GB-4GB.

<details>
<summary>💡 Mẹo tải nhanh (Torrent) — nhấn để xem</summary>

Sử dụng torrent giúp tăng tốc tải về khi server chủ chậm hoặc bị giới hạn băng thông. Nếu bạn sử dụng torrent:

- Dùng client: `qBittorrent` hoặc `Transmission`.
- Luôn kiểm tra SHA256 checksum sau khi tải xong để đảm bảo file không bị giả mạo:

```bash
# Ví dụ kiểm tra SHA256 trên macOS / Linux
sha256sum kali-linux-*.iso
```

**Cẩn trọng:** Chỉ dùng torrent từ trang chính thức của Kali hoặc nguồn uy tín. Không tải ISO từ nguồn lạ.

</details>

---

## 2. Tạo xác máy ảo (Create VM Shell)

Chúng ta cần tạo một cái "vỏ" máy tính ảo trước khi nạp hệ điều hành vào.

1.  Mở VMware, chọn **"Create a New Virtual Machine"**.
2.  Chọn **"Installer disc image file (iso)"** → Bấm **Browse** và chọn file Kali `.iso` bạn vừa tải về.
3.  **Quan trọng:** Ở bước chọn hệ điều hành:
    * **Guest operating system:** Linux
    * **Version:** Debian 10.x 64-bit (hoặc Debian 11/12 64-bit). *Vì Kali dựa trên nền tảng Debian.*
4.  Đặt tên máy ảo (VD: `Kali-CTF-2025`) và chọn nơi lưu trữ.
5.  **Disk Capacity (Ổ cứng):**
    * Chọn tối thiểu **30GB - 40GB** (Kali và các tool CTF ăn khá nhiều dung lượng).
    * Chọn **"Store virtual disk as a single file"** để máy chạy mượt hơn.
6.  Bấm **Customize Hardware**:
    * **Memory (RAM):** Kéo lên 2GB hoặc 4GB.
    * **Processors:** Chọn 2 cores.
    * **Network Adapter:** Chọn **NAT**.
7.  Bấm **Finish**.

---

## 3. Quy trình cài đặt (The Installation Process)

Bây giờ bấm nút **Power on** (Play) để khởi động máy ảo. Màn hình cài đặt của Kali sẽ hiện ra.

### Bước 1: Boot Menu
Dùng phím mũi tên chọn **Graphical install** rồi Enter. (Giao diện đồ họa cho dễ nhìn).

### Bước 2: Ngôn ngữ & Vùng
* **Language:** Chọn *English* (Khuyên dùng tiếng Anh để sau này dễ tra cứu lỗi).
* **Location:** *United States* hoặc *Vietnam* (tùy bạn, không quan trọng lắm).
* **Keyboard:** *American English*.

### Bước 3: Cấu hình Mạng & User
Máy sẽ tự chạy cấu hình mạng một lúc. Sau đó:
1.  **Hostname:** Để mặc định là `kali` hoặc đặt tên ngầu ngầu tùy ý.
2.  **Domain name:** Bỏ trống.
3.  **Full name of new user:** Nhập tên bạn (VD: `hutech-student`).
4.  **Username:** Tên đăng nhập (VD: `hacker`). **Hãy nhớ kỹ cái này!**
5.  **Password:** Đặt mật khẩu. **Nhớ kỹ cái này luôn!**

### Bước 4: Phân vùng ổ cứng (Partitioning) - QUAN TRỌNG
Đây là đoạn người mới hay sợ nhất, nhưng trên máy ảo thì cứ mạnh dạn lên.

1.  Chọn **Guided - use entire disk** (Hướng dẫn tự động - dùng toàn bộ ổ đĩa).
2.  Chọn ổ đĩa ảo VMware vừa tạo (thường là `SCSI3 (0,0,0)`).
3.  Chọn **All files in one partition** (Tất cả trong 1 phân vùng - Khuyên dùng cho người mới).
4.  Chọn **Finish partitioning and write changes to disk**.
5.  Hiện ra thông báo hỏi "Are you sure?" → Chọn **Yes** → Continue.

*(Lúc này đi pha cốc cà phê, quá trình cài đặt hệ thống nền sẽ mất khoảng 5-10 phút)*.

### Bước 5: Chọn phần mềm (Software Selection)
Nó sẽ hỏi bạn muốn cài giao diện nào.
* Để mặc định (đã tích sẵn **Xfce** và **Default recommended tools**).
* Bấm **Continue**.

*(Đoạn này lâu nhất, mất khoảng 10-20 phút để tải và cài các công cụ).*

### Bước 6: Cài đặt GRUB Bootloader

<details>
<summary><strong>GRUB — nhanh (mở để xem)</strong></summary>

GRUB (GRand Unified Bootloader) là chương trình nạp khởi động: được cài vào ổ đĩa hoặc phân vùng EFI để firmware (BIOS/UEFI) biết cách tải kernel và khởi động hệ điều hành.

</details>

Hướng dẫn nhanh (an toàn):

1. Khi hỏi "Install the GRUB boot loader to your primary drive?" → chọn **Yes**.
2.  Ở "Device for boot loader installation" → chọn ổ đĩa chính (ví dụ: `/dev/sda`). **KHÔNG** chọn "Enter device manually" và **KHÔNG** chọn một phân vùng như `/dev/sda1`.

Lưu ý ngắn: Trên VM chọn `/dev/sda` an toàn; trên máy thật hãy cẩn trọng — sai ổ có thể làm mất boot của hệ khác.

### Bước 7: Hoàn tất
Thông báo "Installation is complete". Bấm **Continue**. Máy ảo sẽ tự khởi động lại.

---

## 4. Sau khi cài đặt (Post-Installation)

Đăng nhập bằng User và Password bạn đã tạo ở Bước 3.

### Cài VMware Tools (Để full màn hình & Copy Paste)
Thường thì Kali mới nhất đã tự tích hợp `open-vm-tools`. Nhưng nếu bạn không thể phóng to full màn hình hoặc không copy file từ máy thật sang được, hãy mở Terminal (Ctrl+Alt+T) và chạy lệnh sau:

```bash
sudo apt update
sudo apt install -y open-vm-tools-desktop fuse
reboot