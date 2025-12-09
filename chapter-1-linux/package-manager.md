# 📦 Mission 8: Kho Vũ Khí & Cỗ Máy Thời Gian (Apt & Git)

> **Mở đầu:** Trong thế giới Cyber Security, công cụ (Tools) thay đổi hàng ngày. Hôm nay có lỗ hổng mới, ngày mai đã có tool khai thác (exploit) trên GitHub.
>
>   * **APT** giúp bạn giữ hệ thống Kali luôn sắc bén với các công cụ mới nhất.
>   * **Git** giúp bạn quản lý mã nguồn, tải tool về và quan trọng nhất: **Hợp tác làm việc nhóm** (Teamwork).

-----

## 🛠️ Phần 1: APT - Quản lý gói phần mềm (Advanced Package Tool)

Hệ điều hành Kali Linux dựa trên Debian, và nó sử dụng hệ thống quản lý gói `.deb`. `apt` là công cụ dòng lệnh để quản lý các gói này. Hãy tưởng tượng nó như **CH Play** hay **App Store**, nhưng dành cho Hacker.

### 1\. Cơ chế hoạt động: "Sources.list"

Khi bạn gõ `sudo apt install nmap`, máy tính làm sao biết tải ở đâu?
Nó sẽ nhìn vào file cấu hình: `/etc/apt/sources.list`. Đây là danh sách các địa chỉ kho chứa (Repositories).

> **Lỗi thường gặp:** Nếu bạn không cài được gì cả, khả năng cao file `sources.list` bị lỗi hoặc thiếu. Bạn cần Google "Kali Linux sources list" và paste nội dung chuẩn vào file này.

### 2\. Quy trình Cập nhật chuẩn (The Update Ritual)

Bạn nên chạy combo này thường xuyên:

1.  **`sudo apt update`**:
      * Lệnh này **KHÔNG** cập nhật phần mềm.
      * Nó chỉ tải về **danh sách** các phiên bản mới nhất từ kho chứa về máy (giống như lấy tờ catalogue mới nhất từ siêu thị).
2.  **`sudo apt upgrade`**:
      * Dựa vào danh sách vừa tải, nó sẽ so sánh và nâng cấp các phần mềm đã cài trên máy bạn.
3.  **`sudo apt dist-upgrade`** (Khuyên dùng cho Kali):
      * Mạnh hơn `upgrade`. Nó xử lý thông minh các sự thay đổi về thư viện (dependencies) và cập nhật cả nhân hệ điều hành (Kernel).

### 3\. Các lệnh quản lý chuyên sâu

  * **Tìm kiếm phần mềm:** Bạn muốn cài một tool quay màn hình nhưng không nhớ tên chính xác?
    ```bash
    apt search screen recorder
    ```
  * **Xem thông tin gói:** Xem tool này làm gì trước khi cài.
    ```bash
    apt show nmap
    ```
  * **Gỡ bỏ sạch sẽ (`purge` vs `remove`):**
      * `apt remove tool`: Chỉ xóa file chạy, giữ lại file cấu hình (config).
      * `apt purge tool`: Xóa sạch sành sanh cả file cấu hình (dùng khi muốn reset tool hoàn toàn).
  * **Dọn dẹp rác hệ thống:**
    ```bash
    sudo apt autoremove
    # Xóa các thư viện (libs) được cài kèm theo các phần mềm cũ mà giờ không ai dùng nữa.
    ```

### 🚨 Troubleshooting: Lỗi "Could not get lock"

Khi bạn thấy lỗi: `E: Could not get lock /var/lib/dpkg/lock-frontend`.

  * **Nguyên nhân:** Có một tiến trình cập nhật khác đang chạy ngầm hoặc lần cập nhật trước bị tắt đột ngột.
  * **Cách xử lý:**
    1.  Khởi động lại máy (An toàn nhất).
    2.  Hoặc giết tiến trình đang kẹt: `sudo killall apt apt-get`

-----

## 🐙 Phần 2: Git - Quản lý phiên bản (Version Control System)

Git không chỉ để tải code. Git là hệ thống lưu lại lịch sử thay đổi của file. Nếu bạn lỡ tay xóa code hoặc viết sai logic khiến chương trình sập, Git giúp bạn "quay ngược thời gian".

### 1\. Thiết lập định danh (Làm 1 lần duy nhất)

Để hệ thống biết "Ai là người đã sửa đoạn code này?".

```bash
git config --global user.name "HutechStudent"
git config --global user.email "email_cua_ban@example.com"
```

### 2\. Hai cách bắt đầu dự án

  * **Cách A: `git clone` (Sao chép từ trên mạng)**
    Dùng khi bạn muốn tải source code của người khác (hoặc của team) về máy.

    ```bash
    git clone https://github.com/hacker/tool-khung.git
    cd tool-khung
    ```

  * **Cách B: `git init` (Tạo mới tại máy)**
    Dùng khi bạn bắt đầu một dự án mới tinh trên máy mình.

    ```bash
    mkdir MyProject
    cd MyProject
    git init
    # Lúc này thư mục sẽ có thêm folder ẩn .git
    ```

### 3\. Vòng đời của Code (The Git Workflow)

Hãy nhớ quy trình 3 bước này: **Working** $\rightarrow$ **Staging** $\rightarrow$ **Repository**.

1.  **Sửa file:** Bạn viết code, tạo file mới.
2.  **`git status`**: Kiểm tra xem file nào đã thay đổi (Màu đỏ).
3.  **`git add [tên_file]`** hoặc **`git add .`**: Chọn các file muốn lưu (Chuyển sang màu xanh - Staging Area).
4.  **`git commit -m "Ghi chú thay đổi"`**: Lưu chính thức vào lịch sử. Ghi chú phải rõ ràng (VD: "Fix lỗi login", không ghi "update" chung chung).
5.  **`git push origin main`**: Đẩy code lên Server (GitHub/GitLab) để lưu trữ an toàn.

### 4\. `.gitignore` - Những thứ không được đưa lên

Có những file bạn **tuyệt đối không được up lên Git**, ví dụ: file chứa mật khẩu database, file biên dịch tạm thời, thư mục `node_modules` nặng nề.
$\rightarrow$ Hãy tạo một file tên là `.gitignore` và liệt kê tên các file đó vào. Git sẽ tự động lờ chúng đi.

-----

## ⚔️ Bài tập thực chiến (Grand Lab: Mission 8)

**Kịch bản:** Bạn cần cài đặt công cụ tạo Banner ASCII (để làm đẹp tool của bạn) và lưu một bản mẫu lên Git local.

**Bước 1: Cài đặt vũ khí (`figlet`)**

1.  Cập nhật kho: `sudo apt update`
2.  Cài đặt: `sudo apt install figlet`
3.  Test thử: `figlet "HUTECH CYBER"` (Nhìn ngầu chưa?)

**Bước 2: Khởi tạo kho lưu trữ**

1.  Tạo thư mục dự án:
    ```bash
    mkdir ~/My_CTF_Tool
    cd ~/My_CTF_Tool
    ```
2.  Kích hoạt Git:
    ```bash
    git init
    ```

**Bước 3: Tạo nội dung và Commit**

1.  Tạo banner và lưu vào file text:
    ```bash
    figlet "Project 1" > banner.txt
    ```
2.  Tạo thêm một file script giả lập (dùng lệnh `touch`):
    ```bash
    touch exploit.py
    ```
3.  Kiểm tra trạng thái (Thấy màu đỏ):
    ```bash
    git status
    ```
4.  Thêm tất cả vào vùng chờ (Staging):
    ```bash
    git add .
    ```
5.  Lưu phiên bản đầu tiên:
    ```bash
    git commit -m "Initial commit: Added banner and empty exploit script"
    ```

**Bước 4: Kiểm tra lịch sử**

1.  Xem lại những gì đã làm:
    ```bash
    git log
    ```
    *(Bạn sẽ thấy mã Hash của commit, tác giả và ngày giờ. Đây là bằng chứng công việc của bạn\!)*

-----

**Mission Completed\!**
Bạn đã nắm được cách quản lý phần mềm và mã nguồn.