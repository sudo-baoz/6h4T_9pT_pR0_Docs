# 📝 Mission 6: Cuộc chiến soạn thảo (Nano vs Vim)

> **Bối cảnh:** Bạn đang SSH vào một máy chủ Linux từ xa để vá lỗ hổng bảo mật. Không có chuột, không có VS Code, không có giao diện đồ họa. Chỉ có màn hình đen và con trỏ nhấp nháy. Lúc này, kỹ năng sử dụng **Text Editor (Trình soạn thảo dòng lệnh)** là thứ duy nhất giúp bạn hoàn thành nhiệm vụ.

---

## 🔰 Phần 1: GNU Nano — "Người bạn thân thiện"

**Nano** (Nano's ANOther editor) được thiết kế để thay thế cho Pico, với triết lý: **Đơn giản là trên hết**. Đây là công cụ cứu cánh khi bạn cần sửa nhanh một file config mà không muốn nhớ quá nhiều phím tắt.

---

### 1. Cơ chế hoạt động (Modeless)

Nano hoạt động giống như Notepad trên Windows. Bạn mở lên là gõ được ngay. Không cần chuyển chế độ, không cần thao tác phức tạp.

### 2. Giao diện & Điều hướng

Giao diện Nano chia làm 3 phần:

- **Header:** Hiển thị phiên bản Nano và tên file đang sửa.
- **Body:** Vùng soạn thảo văn bản.
- **Footer (Quan trọng nhất):** Hiển thị các phím tắt (Shortcut). Dấu `^` nghĩa là phím **Ctrl**.

### 3. Bộ phím tắt mở rộng (Cheat Sheet)

Ngoài các lệnh cơ bản, hãy nắm thêm các lệnh sau để làm việc hiệu quả hơn:

| Phím tắt | Chức năng | Ghi chú |
|---|---|---|
| `Ctrl + O` | Lưu file | Hệ thống sẽ hỏi tên file, nhấn `Enter` để xác nhận. |
| `Ctrl + X` | Thoát | Nếu chưa lưu, nó sẽ hỏi `Save modified buffer?` (Y/N). |
| `Ctrl + W` | Tìm kiếm | Nhập từ cần tìm. Bấm `Alt + W` để tìm kết quả tiếp theo. |
| `Ctrl + \` | Thay thế | Tìm và thay thế chuỗi ký tự. |
| `Ctrl + K` | Cắt dòng | Xóa toàn bộ dòng hiện tại và lưu vào bộ nhớ đệm. |
| `Ctrl + U` | Dán dòng | Dán nội dung vừa cắt bằng `Ctrl + K`. |
| `Ctrl + _` | Đến dòng số | Hữu ích khi cần nhảy tới dòng lỗi từ compiler. |
| `Ctrl + C` | Xem vị trí | Hiển thị con trỏ đang ở dòng/cột nào. |

---

## ⚔️ Phần 2: Vim — "Thanh gươm diệt rồng"

**Vim** (Vi Improved) không đơn thuần là editor, nó là một **hệ tư tưởng**. Vim tối ưu hóa việc di chuyển và chỉnh sửa văn bản mà không cần nhấc tay khỏi bàn phím chính (Home row).

---

### 1. Triết lý "Modal" (Đa chế độ)

Khác với Nano, Vim có các "trạng thái" khác nhau. Bạn phải biết mình đang ở chế độ nào, nếu không bạn sẽ kẹt cứng.

#### 🟢 Normal Mode (Chế độ Chỉ huy)

- Đây là chế độ mặc định khi mở Vim.
- Bạn **KHÔNG THỂ** gõ văn bản ở đây.
- Dùng để di chuyển, xóa, copy, paste.
- Bấm **`Esc`** để quay về chế độ này.

#### 🔴 Insert Mode (Chế độ Nhập liệu)

- Dùng để gõ văn bản như bình thường.
- Vào bằng cách bấm **`i`** (insert) hoặc **`a`** (append).
- Góc dưới màn hình sẽ hiện chữ `-- INSERT --`.

#### 🔵 Visual Mode (Chế độ Chọn/Bôi đen)

- Dùng để bôi đen văn bản để copy hoặc xóa hàng loạt.
- Vào bằng phím **`v`**.
- Góc dưới màn hình hiện chữ `-- VISUAL --`.

#### 🟣 Command Mode (Chế độ Lệnh)

- Dùng để Lưu, Thoát, Tìm kiếm nâng cao.
- Từ Normal Mode, bấm **`:`** để bắt đầu nhập lệnh.

---

### 2. Điều hướng kiểu Vim (HJKL)

Tại sao Hacker dùng `h j k l` thay vì mũi tên? Vì chúng nằm ngay hàng phím cơ sở, giúp thao tác nhanh hơn.

- `h`: sang trái
- `j`: xuống
- `k`: lên
- `l`: sang phải

Di chuyển nâng cao:

- `w`: nhảy đến đầu từ tiếp theo
- `e`: nhảy đến cuối từ hiện tại
- `0`: về đầu dòng
- `$`: về cuối dòng
- `gg`: lên đầu file
- `G`: xuống cuối file
- `15G`: nhảy đến dòng 15

---

### 3. Thao tác chỉnh sửa (Editing)

Vim coi thao tác chỉnh sửa như một ngôn ngữ. Cấu trúc: `[Số lượng] + [Hành động] + [Đối tượng]`.

- Xóa (Delete - d): `dd`, `5dd`, `dw`, `d$`.
- Sao chép (Yank - y) & Dán (Paste - p): `yy`, `p`.
- Hoàn tác (Undo/Redo): `u`, `Ctrl + r`.

---

### 4. Tìm kiếm và Thay thế (Search & Replace)

Sức mạnh thực sự của Vim nằm ở phần này — cực kỳ hữu ích khi sửa code.

- Tìm kiếm: `/keyword`, `n` (next), `N` (previous).
- Thay thế toàn file: `:%s/từ_cũ/từ_mới/g`.

Ví dụ: `:%s/Hutech/HutechUniversity/g`.

---

## ⚙️ Phần 3: Tùy biến Vim (`.vimrc`)

Vim mặc định nhìn khá "chán". Bạn có thể biến nó thành một IDE xịn xò bằng cách tạo file cấu hình.

Tạo file cấu hình tại thư mục Home:

```bash
nano ~/.vimrc
```

Dán nội dung này vào để kích hoạt các tính năng hỗ trợ lập trình:

```vim
syntax on
set number
set mouse=a
set tabstop=4
set autoindent
```

Lưu lại và mở Vim lên xem sự khác biệt!

---

## 🔥 Phần 4: Bài tập thực chiến (Scenario Lab)

**Nhiệm vụ:** Bạn cần viết một đoạn script Python đơn giản để quét mạng, nhưng phải dùng **Vim** để thể hiện đẳng cấp.

1. **Khởi tạo:**

```bash
vim network_scanner.py
```

2. **Nhập liệu (Insert Mode):**

- Bấm `i`.
- Gõ đoạn code sau:

```python
import os
print("Bat dau quet mang...")
ip = "192.168.1.1"
response = os.system("ping -c 1 " + ip)
```

- Bấm `Esc` để thoát ra Normal Mode.

3. **Sửa lỗi (Normal Mode):**

- Bạn nhận ra mình muốn quét IP `8.8.8.8` chứ không phải `192.168.1.1`.
- Di chuyển con trỏ đến dòng chứa IP.
- Bấm `dw` hoặc `dd` để xóa dòng cũ.
- Bấm `O` (chữ O hoa) để chèn dòng mới phía trên, hoặc `o` để chèn phía dưới.
- Gõ lại: `ip = "8.8.8.8"`.

4. **Sao chép (Visual Mode):**

- Di chuyển đến dòng `print`.
- Bấm `yy` (copy).
- Di chuyển xuống cuối file (`G`).
- Bấm `p` (paste) để in thêm dòng đó lần nữa.
- Sửa nội dung dòng cuối thành "Ket thuc quet.".

5. **Lưu và Thoát:** `:wq` → Enter.

6. **Chạy thử:**

```bash
python3 network_scanner.py
```

---

**Tổng kết:**

- Dùng **Nano** khi bạn cần sự nhanh gọn, sửa config nhỏ.
- Học **Vim** để trở thành "bậc thầy" dòng lệnh, thao tác trên server lớn và xử lý code phức tạp.
