# 🕵️‍♂️ Mission 21: Digital Forensics - Truy Vết Dữ Liệu

> **📜 Mission Brief:** Digital Forensics (Điều tra số) là khoa học về việc tìm kiếm, khôi phục và phân tích dữ liệu trong các thiết bị điện tử để làm bằng chứng pháp lý.
>
> Nhiệm vụ của bạn không phải là hack vào máy, mà là mổ xẻ cái máy đã chết (Dead Analysis) để trả lời 3 câu hỏi: **Ai làm? Làm khi nào? Và làm như thế nào?**

-----

## 🛑 MODULE 1: NGUYÊN TẮC BẤT DI BẤT DỊCH

Trước khi chạm vào bàn phím, bạn phải thuộc nằm lòng "Lời thề Hippocrates" của dân Forensics:

### 1\. Không bao giờ làm việc trên bằng chứng gốc (Original Evidence)

  * **Tại sao?** Nếu bạn lỡ tay xóa nhầm 1 file hoặc làm thay đổi ngày giờ (timestamp) của file gốc, bằng chứng đó sẽ bị coi là **vô giá trị trước tòa**.
  * **Giải pháp:** Luôn tạo ra một bản sao (Image) chính xác từng bit (Bit-by-bit copy) và chỉ làm việc trên bản sao đó.

### 2\. Sử dụng Write Blocker (Thiết bị chặn ghi)

  * Khi cắm ổ cứng của tội phạm vào máy điều tra, Windows/Linux thường có thói quen tự động ghi các file log hoặc thay đổi metadata.
  * Bạn cần một thiết bị phần cứng (hoặc phần mềm) để đảm bảo: **Dữ liệu chỉ đi ra, không bao giờ đi vào.**

![Image of hardware write blocker connecting to a hard drive](https://upload.wikimedia.org/wikipedia/commons/8/8e/Portable_forensic_tableau.JPG)

-----

## 🏗️ MODULE 2: QUY TRÌNH 4 BƯỚC (THE 4 A's)

Đây là quy trình chuẩn quốc tế (NIST) để xử lý một vụ việc.

### 1\. Thu thập (Acquisition)

Sao chép dữ liệu từ ổ cứng, RAM, USB.

  * **Công cụ:** `dd` (Linux), FTK Imager (Windows).
  * **Lệnh `dd` huyền thoại:**
    ```bash
    # Sao chép toàn bộ ổ đĩa sda sang file ảnh disk.img
    sudo dd if=/dev/sda of=/path/to/evidence/disk.img status=progress
    ```

### 2\. Bảo toàn (Preservation) - Dấu vân tay số

Làm sao chứng minh file ảnh `disk.img` bạn đang giữ giống hệt 100% với ổ cứng của tội phạm?
→ Sử dụng **Hash (Băm)**.

  * Ngay sau khi copy xong, phải tính mã Hash (MD5 hoặc SHA256).
  * Nếu 1 năm sau, mã Hash thay đổi dù chỉ 1 ký tự → Bằng chứng đã bị giả mạo.

### 3\. Phân tích (Analysis)

Dùng công cụ để tìm kiếm:

  * File đã bị xóa (Deleted files).
  * Lịch sử duyệt web.
  * Email, Chat log.
  * Metadata của ảnh (GPS, ngày chụp).

### 4\. Báo cáo (Reporting)

Viết lại quá trình điều tra sao cho một người không biết về IT (như Luật sư, Thẩm phán) cũng hiểu được.

-----

## 🛠️ MODULE 3: KHO VŨ KHÍ ĐIỀU TRA (THE TOOLKIT)

Chúng ta sẽ làm quen với bộ đôi quyền lực nhất: **The Sleuth Kit (TSK)** và **Autopsy**.

### 1\. The Sleuth Kit (TSK) - Sức mạnh dòng lệnh

Đây là bộ thư viện mã nguồn mở dùng để phân tích ổ đĩa (NTFS, FAT, EXT4). Nó chạy bằng dòng lệnh, rất mạnh nhưng khó dùng.

### 2\. Autopsy - Giao diện thân thiện

Autopsy thực chất là giao diện đồ họa (GUI) của Sleuth Kit. Nó được tích hợp sẵn trong Kali Linux.

  * **Tính năng:** Tự động quét và phân loại: "Đây là ảnh", "Đây là file Excel", "Đây là file đã xóa".
  * **Biểu tượng:** Chú chó săn (Sleuth Dog).

-----

## 🧪 MODULE 4: THỰC HÀNH KHÔI PHỤC DỮ LIỆU (LAB)

**Kịch bản:** Một nhân viên đã ăn cắp bí mật công ty, lưu vào file ảnh `bi_mat.jpg`, sau đó xóa file đi và format USB hòng phi tang.
Nhiệm vụ của bạn là tìm lại file đó.

Chúng ta sẽ dùng một công cụ nhỏ gọn hơn Autopsy cho bài tập này: **Foremost** hoặc **Scalpel** (Chuyên gia phục hồi file - File Carving).

### Bước 1: Chuẩn bị hiện trường giả

1.  Tạo một thư mục giả lập USB.
2.  Copy một ảnh vào đó.
3.  Xóa ảnh đó đi (`rm bi_mat.jpg`).

### Bước 2: Cài đặt Foremost

```bash
sudo apt update && sudo apt install foremost -y
```

### Bước 3: Phục hồi (Carving)

Foremost sẽ quét toàn bộ ổ đĩa (hoặc file image), tìm các "dấu hiệu đầu và đuôi" (Header/Footer) của file JPG để khâu chúng lại.

```bash
# Giả sử disk.img là file ảnh của cái USB
foremost -i disk.img -t jpg -o /root/recovered_data
```

  * `-i`: Input file (Bằng chứng).
  * `-t`: Type (Loại file cần tìm, ở đây là jpg).
  * `-o`: Output folder (Nơi lưu file cứu được).

### Bước 4: Kiểm tra kết quả

Vào thư mục `/root/recovered_data/jpg/`. Bạn sẽ thấy file ảnh "đã chết" sống lại\!

> **💡 Nguyên lý:** Khi bạn xóa file (Delete), máy tính chỉ xóa "mục lục" chỉ đường tới file đó. Dữ liệu thực tế vẫn nằm nguyên trên ổ cứng cho đến khi bị dữ liệu khác ghi đè lên. Foremost dựa vào điều này để cứu dữ liệu.

-----

## 🕵️ PRO TIP: TIMELINE ANALYSIS (DÒNG THỜI GIAN)

Trong Autopsy, tính năng mạnh nhất là **Timeline**.
Nó vẽ ra biểu đồ hành vi của người dùng:

  * *8:00 AM:* Cắm USB.
  * *8:05 AM:* Mở file "luong\_giam\_doc.xls".
  * *8:10 AM:* Truy cập Gmail.
  * *8:15 AM:* Xóa file.

→ Nhìn vào Timeline, bạn có thể dựng lại toàn bộ câu chuyện vụ án.

-----

**Mission Completed\!**
Bạn đã biết cách "gọi hồn" dữ liệu.