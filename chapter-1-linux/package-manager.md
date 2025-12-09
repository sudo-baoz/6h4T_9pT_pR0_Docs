# 📦 Mission 8: Cài đặt đồ chơi (Apt & Git)

### (KHO VŨ KHÍ & CỖ MÁY THỜI GIAN)

> **📝 Mission Log:** Trong thế giới Cyber Security, công cụ (Tools) là vũ khí, và Mã nguồn (Source Code) là tài sản.
>
>   * **APT**: Giúp bạn tiếp tế đạn dược, vũ khí mới nhất từ kho tổng hành dinh.
>   * **GIT**: Cho phép bạn quản lý dòng thời gian, quay ngược quá khứ khi lỡ tay phá hỏng hệ thống.

-----

## 🛠️ PHẦN 1: APT SYSTEM (Quản Lý Kho Vũ Khí)

Kali Linux dựa trên Debian, sử dụng hệ thống quản lý gói `.deb`. Hãy coi **APT** (Advanced Package Tool) như một **Chợ Đen** nơi bạn có thể tải về mọi công cụ hack miễn phí.

### 1\. 📡 Quy trình "Nạp đạn" (Update Ritual)

Trước khi cài đặt bất cứ thứ gì, hãy chạy combo lệnh thần thánh này để đồng bộ hóa dữ liệu với máy chủ toàn cầu.

```bash
sudo apt update && sudo apt upgrade -y
```

| Lệnh | Ý nghĩa thực chiến |
| :--- | :--- |
| `sudo apt update` | **Trinh sát:** Chỉ tải về *danh sách* các phần mềm mới nhất (Catalog). Chưa cài gì cả. |
| `sudo apt upgrade` | **Nâng cấp:** Dựa vào danh sách trên, tiến hành nâng cấp toàn bộ vũ khí cũ trong máy lên bản mới nhất. |
| `-y` | **Auto-Confirm:** Tự động chọn "Yes" (đỡ phải bấm `y` mỏi tay). |

### 2\. ⚙️ Thao tác Quản lý (Command Center)

| Chức năng | Câu lệnh (Command) | Ghi chú |
| :--- | :--- | :--- |
| **Cài đặt** | `sudo apt install <tên_tool>` | VD: `sudo apt install nmap` |
| **Tìm kiếm** | `apt search <từ_khóa>` | Khi bạn quên tên chính xác của tool. |
| **Gỡ bỏ** | `sudo apt remove <tên_tool>` | Chỉ xóa phần mềm, giữ lại cấu hình. |
| **Hủy diệt** | `sudo apt purge <tên_tool>` | Xóa sạch sẽ không còn dấu vết (Config + Data). |
| **Dọn rác** | `sudo apt autoremove` | Xóa các thư viện thừa thãi (Dependencies). |

> **⚠️ WARNING:** Nếu gặp lỗi `Could not get lock...`, nghĩa là hệ thống đang cập nhật ngầm. Hãy khởi động lại máy hoặc chờ 5 phút\!

-----

## 🐙 PHẦN 2: GIT CONTROL (Kiểm Soát Dòng Thời Gian)

**Git** không chỉ là công cụ tải code. Nó là **Save Point** (Điểm lưu game) cho lập trình viên.

### 1\. 📥 Clone (Sao chép bí kíp)

Lệnh dùng nhiều nhất để lấy tool từ GitHub về Kali.

```bash
git clone https://github.com/sqlmapproject/sqlmap.git
# Tải công cụ SQLMap về máy ngay lập tức
```

### 2\. 🔄 Vòng đời Code (The Workflow)

Hãy tưởng tượng quy trình lưu code như việc đóng gói hàng hóa:

1.  **WORKING DIR** 🛠️: Bạn đang viết code (File màu đỏ - chưa theo dõi).
2.  **STAGING AREA** 📦: Bạn chọn file để chuẩn bị đóng gói (`git add`).
3.  **REPOSITORY** 💾: Bạn dán tem niêm phong và lưu vào kho (`git commit`).

### 3\. 🕹️ Các lệnh sinh tồn

  * **Bước 1: Điểm danh file**

    ```bash
    git status
    # Luôn gõ lệnh này để biết file nào đang thay đổi
    ```

  * **Bước 2: Chọn file cần lưu**

    ```bash
    git add .
    # Dấu chấm (.) nghĩa là chọn TẤT CẢ file trong thư mục hiện tại
    ```

  * **Bước 3: Lưu lại (Chụp ảnh)**

    ```bash
    git commit -m "Fixed login bug"
    # -m: Message (Ghi chú). Hãy viết tiếng Anh hoặc tiếng Việt không dấu cho chuyên nghiệp.
    ```

  * **Bước 4: Đẩy lên mây (Upload)**

    ```bash
    git push origin main
    # Đưa code lên GitHub/GitLab
    ```

-----

## ⚔️ GRAND LAB: CHIẾN DỊCH "BANNER HACKER"

**Nhiệm vụ:** Cài đặt công cụ tạo chữ nghệ thuật và dùng Git để lưu lại tác phẩm.

### 🟢 Phase 1: Trang bị (Install)

Cài đặt `figlet` - công cụ tạo Banner ASCII siêu ngầu.

```bash
sudo apt update
sudo apt install figlet -y
```

👉 **Test:** Gõ `figlet HUTECH` xem điều gì xảy ra?

### 🟡 Phase 2: Khởi tạo căn cứ (Init)

Tạo thư mục dự án và biến nó thành kho Git.

```bash
mkdir ~/CyberProject      # Tạo thư mục
cd ~/CyberProject         # Đi vào bên trong
git init                  # Kích hoạt chế độ theo dõi của Git
```

### 🔴 Phase 3: Thực thi & Lưu trữ (Commit)

1.  **Tạo Banner:**
    ```bash
    figlet "Cyber Security" > banner.txt
    ```
2.  **Kiểm tra:**
    ```bash
    cat banner.txt
    git status           # Thấy file banner.txt màu đỏ chứ?
    ```
3.  **Lưu vào kho:**
    ```bash
    git add .
    git commit -m "Add cool banner"
    ```
4.  **Kiểm tra lịch sử:**
    ```bash
    git log
    ```

-----

### 🏆 MISSION DEBRIEF (Tổng kết)

  * [x] Bạn đã biết cách **Cập nhật hệ thống** để vá lỗ hổng.
  * [x] Bạn đã biết cách **Cài đặt tool** bằng `apt`.
  * [x] Bạn đã hiểu quy trình **Save game** bằng `git`.