# 🕵️‍♂️ MISSION 1: KALI LINUX — VŨ KHÍ TỐI THƯỢNG CHO CTF

<p align="center">
	<img src="https://img.shields.io/badge/CTF-Kali-blue?style=for-the-badge&logo=Kali-Linux" alt="Kali badge" />
	<img src="https://img.shields.io/badge/Tools-600%2B-orange?style=for-the-badge" alt="tools badge" />
</p>

> *"The quieter you become, the more you are able to hear."* — Kali mindset

Trong thế giới An toàn thông tin (ATTT) và đặc biệt là các cuộc thi Capture The Flag (CTF), môi trường làm việc không chỉ là một hệ điều hành — đó là vùng tác chiến. Kali Linux là một trong những distro chuẩn mực cho cộng đồng pentest/CTF bởi tính đầy đủ, chuẩn hóa và khả năng cập nhật nhanh.

---

## 🚀 Tại sao chọn Kali? (Short Answer)

- **Out-of-the-box:** cài sẵn hàng trăm công cụ phổ biến (đỡ tốn thời gian setup).
- **Rolling release:** cập nhật nhanh, tiện khi cần tool/patch mới.
- **Ecosystem mạnh:** nhiều write-up, tutorial và cộng đồng hỗ trợ.

---

## ⚖️ So sánh nhanh: Kali vs Ubuntu vs Parrot

| Tiêu chí | Kali Linux | Ubuntu / Mint | Parrot OS |
|---|---:|:---:|:---:|
| Mục tiêu | Offensive Security / CTF | Desktop / Dev | Privacy + Pentest |
| Quyền mặc định | root-first (cần cẩn trọng) | user-first (an toàn hơn) | user-friendly hơn Kali |
| Tools | ~600+ preinstalled | Cần cài thêm | Nhiều công cụ + privacy tools |
| Dùng cho | Pentesters, CTFers | Developers, daily use | Pentesters muốn nhẹ nhàng |

> Kết luận ngắn: dùng Kali cho lab/CTF; dùng Ubuntu cho work/daily.

---

## 🧰 Nhóm công cụ quan trọng (tập trung học)

Để dễ đọc và nhớ, tôi chia mỗi nhóm thành khối riêng — mỗi khối có 1 câu mô tả, 1 ví dụ lệnh nhanh và 1 dòng "Khi nào dùng".

### 🔎 Information Gathering (Recon)
- Mục đích: tìm host sống, port mở và dịch vụ để lập scope cho cuộc chơi.
- Ví dụ nhanh: `nmap -sV target.example.com`
- Khi nào dùng: ngay ở bước đầu của mọi challenge (recon).

---

### 🌐 Network Analysis
- Mục đích: bắt và phân tích gói tin để hiểu lưu lượng, session và payload.
- Ví dụ nhanh: `sudo tcpdump -i eth0 -c 200 -w capture.pcap` (mở bằng Wireshark để phân tích).
- Khi nào dùng: debug kết nối, phân tích traffic hoặc bài Forensics.

---

### 🕸️ Web Pentest
- Mục đích: khám phá endpoints, dò thư mục và tìm injection/logic bugs trên ứng dụng web.
- Ví dụ nhanh: `gobuster dir -u https://target -w /usr/share/wordlists/dirb/common.txt`
- Khi nào dùng: khi target là web app hoặc có endpoint HTTP(S).

---

### 🔐 Passwords & Cracking
- Mục đích: xử lý hash, brute-force, và khôi phục mật khẩu (chỉ trên lab hoặc dữ liệu được phép xử lý).
- Ví dụ nhanh: `john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt`
- Khi nào dùng: khi bạn thu thập được hash hoặc cần kiểm thử mật khẩu weak.

---

### 🐛 Reverse Engineering & Pwning

<details>
<summary>Ghi chú</summary>

_Ghi chú:_ Bạn không cần học 600 tool; tập trung vào 15–25 công cụ chính sẽ giúp bạn giải được phần lớn challenge.

</details>

_Ghi chú:_ Bạn không cần học 600 tool; tập trung vào 15–25 công cụ chính sẽ giúp bạn giải được phần lớn challenge.
---

## Lộ trình học & Tiêu chí ưu tiên (gợi ý nhanh)

1. Bắt đầu với `nmap`, `netcat`, `tcpdump` để hiểu mạng.
2. Học `burpsuite`, `gobuster` cho Web basics.
3. Thử `john`/`hashcat` cho password cracking basics.
4. Sang `gdb`/`ghidra` nếu muốn học Pwn/Reverse.

_Ghi chú về đạo đức:_ luôn làm lab trong môi trường riêng (VM/local network) và xin phép khi thử nghiệm trên hệ thống không phải của bạn.

---

## 💡 Pro-tips & setup nhanh

- Không dùng Kali làm máy chính; chạy trong VM (VirtualBox / VMware) hoặc container.
- Luôn tạo snapshot trước khi update hoặc thử script lạ.
- Cài `zsh` + `oh-my-zsh` và vài alias thường dùng để tiết kiệm thời gian.

Kiểm tra SHA256 của ISO (luôn kiểm tra trước khi cài):

```bash
sha256sum kali-linux-*.iso
# So sánh kết quả với checksum chính thức từ kali.org
```

---

## 🔒 Vấn đề đạo đức & pháp lý (không thể nhắc lại đủ lần)

Mọi kỹ thuật trong cuốn sách này là cho mục đích giáo dục. Tuyệt đối **không** sử dụng các kỹ thuật để xâm nhập trái phép. Luôn có giấy phép bằng văn bản khi thử nghiệm trên hệ thống không phải của bạn.

---

## 📚 Tài nguyên hữu ích

- Kali downloads: https://www.kali.org/get-kali/
- Official docs: https://www.kali.org/docs/
- TryHackMe / PicoCTF / HackTheBox — nơi luyện challenge.

---

## 🎯 Next mission

Bạn đã hiểu tại sao chọn Kali. Bước tiếp theo: thiết lập Lab (VM vs WSL2), cấu hình dotfiles và cài một số tool cơ bản để sẵn sàng giải challenge.
