# 💥 Mission 25: Buffer Overflow (BOF) - Tràn Bộ Đệm Cơ Bản

> **📜 Mission Brief:** Hãy tưởng tượng bạn có một cái ly chỉ chứa được 200ml nước. Nếu bạn cố đổ vào 500ml, nước sẽ **tràn ra ngoài** làm ướt bàn phím, giấy tờ bên cạnh.
>
> Trong máy tính, "cái ly" là biến (**Buffer**). Nếu Lập trình viên không kiểm tra kích thước dữ liệu đầu vào, Hacker có thể nhồi quá nhiều dữ liệu, làm nó tràn sang các vùng nhớ quan trọng khác, cho phép Hacker chiếm quyền điều khiển CPU.

-----

## 🛑 KHU VỰC CẤM: TUYỆT ĐỐI CẨN TRỌNG

> **⚠️ WARNING:** Kỹ thuật này can thiệp sâu vào bộ nhớ hệ thống.
>
>   * Chỉ thực hành trên các bài Lab được thiết kế sẵn (như *Protostar*, *Pwnable.kr* hoặc code C tự viết).
>   * Không thử trên các phần mềm đang chạy thật trên máy, có thể gây màn hình xanh (BSOD) hoặc treo hệ điều hành.

-----

## 🏗️ MODULE 1: GIẢI PHẪU BỘ NHỚ (THE STACK)

Để hiểu BOF, bạn phải hiểu **Stack**. Stack là nơi chứa các biến cục bộ khi một hàm chạy. Nó xếp chồng lên nhau như chồng đĩa.

Cấu trúc quan trọng của một hàm trong Stack (từ địa chỉ thấp đến cao):

1.  **Buffer:** Biến chứa dữ liệu (Ví dụ: `char ten[64]`).
2.  **Saved EBP:** Con trỏ khung (Base Pointer).
3.  **Return Address (EIP/RIP):** **TỬ HUYỆT CỦA CHƯƠNG TRÌNH.**
      * Nó chứa địa chỉ của dòng lệnh tiếp theo mà CPU phải nhảy về sau khi hàm chạy xong.
      * **Mục tiêu của Hacker:** Ghi đè lên địa chỉ này để bắt CPU nhảy đến nơi Hacker muốn (chạy Shellcode).

-----

## ☠️ MODULE 2: CƠ CHẾ TẤN CÔNG (THE CRASH)

Hãy xem đoạn code C cực kỳ nguy hiểm sau:

```c
#include <stdio.h>
#include <string.h>

void vulnerable_function(char *str) {
    char buffer[64];        // Chỉ cấp 64 byte
    strcpy(buffer, str);    // Lỗi: Copy không kiểm tra độ dài!
}

int main(int argc, char *argv[]) {
    vulnerable_function(argv[1]);
    return 0;
}
```

**Kịch bản khai thác:**

1.  **Input \< 64 ký tự:** Chương trình chạy bình thường.
2.  **Input = 100 ký tự "A":**
      * 64 chữ "A" đầu lấp đầy `buffer`.
      * Các chữ "A" tiếp theo tràn ra, ghi đè lên `Saved EBP`.
      * 4 chữ "A" tiếp theo ghi đè lên **Return Address (EIP)**.

**Hậu quả:** Khi hàm kết thúc, nó bảo CPU: *"Hãy nhảy tới địa chỉ `0x41414141` (AAAA) để chạy tiếp"*. Vì địa chỉ này không tồn tại hoặc không hợp lệ → Chương trình sập (**Segmentation Fault**).

-----

## 🛠️ MODULE 3: CHẾ TẠO PAYLOAD (SHELLCODE)

Hacker không chỉ muốn làm sập chương trình. Họ muốn chạy `cmd.exe` hoặc `/bin/sh`.
Để làm điều đó, Payload phải có cấu trúc như chiếc bánh kẹp:

### Cấu trúc Payload chuẩn:

`[NOP Sled] + [Shellcode] + [Return Address]`

1.  **NOP Sled (Trượt tuyết):**
      * Lệnh `0x90` (No Operation - Không làm gì cả).
      * Khi CPU nhảy vào vùng này, nó sẽ trượt đi vèo vèo cho đến khi chạm vào Shellcode. Giúp tăng tỉ lệ trúng đích.
2.  **Shellcode:**
      * Là đoạn mã máy (Assembly) nhỏ gọn dùng để mở Command Prompt.
      * Ví dụ (Linux x86): `\x31\xc0\x50\x68\x2f\x2f\x73\x68...`
3.  **Return Address (Địa chỉ trả về):**
      * Hacker sẽ tính toán và ghi đè EIP bằng địa chỉ trỏ vào vùng NOP Sled ở trên.

**Ví dụ Payload (Python):**

```python
# Tạo payload 100 bytes
padding = b"A" * 76          # Lấp đầy buffer + EBP
ret_addr = b"\xef\xbe\xad\xde" # Địa chỉ trỏ về Stack (Ví dụ: 0xDEADBEEF)
shellcode = b"\xcc" * 20     # (Giả lập shellcode)

payload = padding + ret_addr + shellcode
print(payload)
```

-----

## 🧪 MODULE 4: THỰC HÀNH LAB (AN TOÀN)

Chúng ta sẽ thử làm sập chương trình (Crash).

1.  **Biên dịch code lỗi:**
    ```bash
    # Tắt chế độ bảo vệ stack (Canary) để dễ hack
    gcc -fno-stack-protector -z execstack -o vuln vuln.c
    ```
2.  **Fuzzing (Thử nghiệm):**
      * Chạy: `./vuln AAAA` → OK.
      * Chạy: `./vuln $(python3 -c "print('A'*50)")` → OK.
      * Chạy: `./vuln $(python3 -c "print('A'*80)")` → **Segmentation fault (core dumped)**.
3.  **Kiểm tra bằng GDB:**
      * Load GDB: `gdb ./vuln`
      * Chạy: `r $(python3 -c "print('A'*80)")`
      * Nhìn vào thanh ghi **EIP/RIP**: Bạn sẽ thấy nó chứa toàn `0x41414141`.
      * → **Bạn đã chiếm quyền điều khiển con trỏ lệnh!**

-----

## 🛡️ MODULE 5: PHÒNG THỦ (MODERN DEFENSE)

Ngày nay, hack BOF khó hơn nhiều vì OS có các lớp giáp:

1.  **Stack Canary (Chim hoàng yến):**
      * Đặt một giá trị ngẫu nhiên (Canary) trước Return Address.
      * Trước khi thoát hàm, kiểm tra xem Canary có bị thay đổi không. Nếu có → Hacker đang ghi đè → Dừng chương trình ngay.
2.  **DEP / NX (No-Execute):**
      * Đánh dấu vùng nhớ Stack là "Chỉ chứa dữ liệu, không được chạy code".
      * Nếu CPU cố chạy Shellcode trên Stack → Chặn ngay.
3.  **ASLR (Address Space Layout Randomization):**
      * Mỗi lần chạy, địa chỉ bộ nhớ thay đổi ngẫu nhiên. Hacker không biết địa chỉ nào để nhảy tới.

### ✅ Cách code an toàn:

Tuyệt đối không dùng các hàm không kiểm soát độ dài (`strcpy`, `gets`, `strcat`).
Hãy dùng các hàm an toàn (`strncpy`, `fgets`, `strncat`).

-----

**Mission Completed\!**
Bạn đã chạm tay vào "Thánh địa" của Hacker mũ đen.