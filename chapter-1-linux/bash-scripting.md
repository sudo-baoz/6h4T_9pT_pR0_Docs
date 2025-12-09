
# 🤖 MISSION 7: TỰ ĐỘNG HÓA VỚI BASH SCRIPT (CƠ BẢN)

<p align="center">
    <img src="https://img.shields.io/badge/Bash-Automation-yellow?style=for-the-badge&logo=gnu-bash"/>
    <img src="https://img.shields.io/badge/CTF-Speed-red?style=for-the-badge"/>
</p>

> *"Don't repeat yourself (DRY)."* — Nếu bạn gõ một lệnh >2 lần, script hóa nó.

---

## 🧬 1. Giải Phẫu Một File Bash Script

Một Bash script là file văn bản chứa các lệnh được thực thi tuần tự. Dòng đầu thường là Shebang để định nghĩa trình thông dịch.

### 📌 Shebang (`#!`)

- `#!/bin/bash` — cứng; trỏ trực tiếp tới bash.
- `#!/usr/bin/env bash` — khuyên dùng: linh hoạt, tìm `bash` theo `PATH`.

### 📝 Ví dụ: Hello World (`hello.sh`)

```bash
#!/usr/bin/env bash

# Comment: dòng này bị bỏ qua khi chạy
echo "🔥 Hệ thống đã khởi động!"
echo "Đang chuẩn bị vũ khí cho CTF..."
```

---

## 📦 2. Biến (Variables) - Chiếc Hộp Lưu Trữ

Thay vì lặp IP/port/wordlist, dùng biến. Lưu ý: không để khoảng trắng quanh dấu `=`.

```bash
#!/usr/bin/env bash

# Khai báo
TARGET_IP="192.168.1.105"
PORT="80"
TOOL="nmap"

echo "Đang sử dụng $TOOL để quét $TARGET_IP:$PORT"
```

---

## 🔄 3. Vòng Lặp (Loops) - Sức Mạnh Của Số Lượng

Vòng lặp cho phép tự động hoá nhiều tác vụ lặp lại.

```bash
for i in {1..5}; do
    echo "Đây là payload số $i" > "payload_$i.txt"
    echo "✅ Đã tạo payload_$i.txt"
done
```

---

## ⚡ 4. Quyền Thực Thi (Execution Permissions)

1. Kiểm tra quyền: `ls -l autoscan.sh` (tìm chữ `x`).
2. Cấp quyền: `chmod +x autoscan.sh`.
3. Chạy: `./autoscan.sh` (dấu `./` nghĩa là file tại thư mục hiện tại).

---

> **Pro tip:** Khi viết script, thêm `set -euo pipefail` ở đầu để script dừng khi lỗi, không dùng biến chưa khai báo và bắt lỗi trong pipeline.

---

## ⚔️ BÀI TẬP THỰC CHIẾN (MINI CTF LAB)

Đừng chỉ đọc — mở Terminal và thử ngay.

### 🟢 Level 1: The Informer

Tạo `info.sh`:
- Nhiệm vụ: in `Xin chào, tôi đang đăng nhập với user: [Tên_User]`.
- Gợi ý: dùng `whoami` hoặc `$(whoami)`.

### 🟡 Level 2: The Log Collector

Tạo `backup_logs.sh`:
- Nhiệm vụ:
    1. Tạo thư mục `evidence`.
    2. Copy file ví dụ `/etc/passwd` vào `evidence`.
    3. Đổi tên file thành `passwd.bak`.
    4. In `Đã thu thập dữ liệu thành công!`.

### 🔴 Level 3: Simple Ping Sweeper (Recon Tool)

Tạo `pingsweep.sh` để quét các host sống trong subnet.

```bash
#!/usr/bin/env bash
SUBNET="192.168.1" # sửa theo mạng của bạn

echo "📡 Đang quét mạng $SUBNET.0/24..."
for ip in {1..20}; do
    if ping -c 1 -W 1 "$SUBNET.$ip" > /dev/null; then
        echo "Host $SUBNET.$ip đang hoạt động! 🟢"
    else
        echo "Host $SUBNET.$ip không phản hồi. 🔴"
    fi
done
```

**Giải thích:** nhanh, nhẹ, hữu dụng khi bạn không muốn cài Nmap.

---

## 🔐 Solutions (ẩn mở khi cần)

<details>
<summary><strong>Show solutions</strong></summary>

**Level 1 — info.sh**

```bash
#!/usr/bin/env bash
user=$(whoami)
echo "Xin chào, tôi đang đăng nhập với user: $user"
```

**Level 2 — backup_logs.sh**

```bash
#!/usr/bin/env bash
mkdir -p evidence
cp /etc/passwd evidence/ || { echo "Không copy được /etc/passwd (quyền)"; exit 1; }
mv evidence/passwd evidence/passwd.bak
echo "Đã thu thập dữ liệu thành công!"
```

**Level 3 — pingsweep.sh**

```bash
#!/usr/bin/env bash
SUBNET="192.168.1"
for ip in {1..20}; do
    if ping -c 1 -W 1 "$SUBNET.$ip" > /dev/null; then
        echo "Host $SUBNET.$ip đang hoạt động! 🟢"
    fi
done
```

</details>

---

> **🎯 Next Mission:** Mission 8 — Networking 101 cho Hacker.