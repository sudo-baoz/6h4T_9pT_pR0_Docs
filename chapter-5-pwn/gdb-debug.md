# 🐛 Mission 24: GDB & Pwndbg - Soi Code Tận "Xương Tủy"

> **📜 Mission Brief:** GDB (GNU Debugger) là công cụ gỡ lỗi tiêu chuẩn của Linux. Nó cho phép bạn dừng chương trình tại bất kỳ thời điểm nào để kiểm tra nội dung bộ nhớ.
>
> Tuy nhiên, GDB gốc rất... xấu và khó dùng. Vì vậy, Hacker luôn cài thêm **Pwndbg** (hoặc GEF) - một plugin giúp hiển thị màu sắc, thanh ghi và Stack một cách trực quan.

-----

## 🛠️ MODULE 1: TRANG BỊ VŨ KHÍ (INSTALLATION)

Trước tiên, hãy biến cái GDB nhàm chán thành một bảng điều khiển Hacker xịn xò.

### 1\. Cài đặt GDB

```bash
sudo apt update && sudo apt install gdb -y
```

### 2\. Cài đặt Pwndbg (Plugin)

```bash
git clone https://github.com/pwndbg/pwndbg
cd pwndbg
./setup.sh
```

*Sau khi cài xong, mỗi lần gõ `gdb`, Pwndbg sẽ tự động kích hoạt.*

-----

## 🖥️ MODULE 2: GIAO DIỆN PWNDBG (THE HUD)

Khi bạn chạy một chương trình trong GDB+Pwndbg, màn hình sẽ chia làm 4 khu vực (Context) cực kỳ quan trọng:

1.  **REGISTERS (Thanh ghi):** Hiển thị giá trị hiện tại của các thanh ghi CPU (`RAX`, `RBX`, `RIP`...).
      * **Quan trọng nhất:** `RIP` (Instruction Pointer) - Con trỏ lệnh, cho biết dòng lệnh nào sắp được thực thi tiếp theo.
2.  **DISASM (Dịch ngược):** Hiển thị mã Assembly đang chạy. Mũi tên `►` chỉ vào lệnh hiện tại.
3.  **STACK (Ngăn xếp):** Hiển thị nội dung bộ nhớ Stack (nơi chứa biến cục bộ, địa chỉ trả về).
4.  **BACKTRACE:** Lịch sử các hàm đã gọi (Hàm A gọi Hàm B, Hàm B gọi Hàm C...).

-----

## 🎮 MODULE 3: BỘ ĐIỀU KHIỂN (COMMAND CHEAT SHEET)

Hãy tưởng tượng bạn đang xem một bộ phim quay chậm. Bạn có các nút điều khiển sau:

| Lệnh GDB | Viết tắt | Ý nghĩa | Giải thích game thủ |
| :--- | :--- | :--- | :--- |
| `file <ten_file>` | | Nạp chương trình | "Load Game" |
| `run` | `r` | Chạy chương trình | "Start Game" |
| `break *0x...` | `b` | Đặt điểm dừng | "Pause tại cảnh này" |
| `continue` | `c` | Chạy tiếp đến điểm dừng sau | "Resume" |
| `next instruction` | `ni` | Chạy qua 1 dòng lệnh Assembly | "Tua qua 1 dòng" (Không chui vào hàm con) |
| `step instruction` | `si` | Chạy vào 1 dòng lệnh Assembly | "Chui vào hàm con xem chi tiết" |
| `quit` | `q` | Thoát | "Alt + F4" |

-----

## 🛑 MODULE 4: BREAKPOINTS & WATCHPOINTS (CÁI BẪY)

Làm sao để dừng chương trình đúng chỗ ta muốn?

### 1\. Breakpoints (Điểm dừng tĩnh)

Dừng lại khi code chạy đến một địa chỉ hoặc một hàm cụ thể.

  * **Đặt bẫy:** `b main` (Dừng ngay khi hàm main bắt đầu).
  * **Đặt theo địa chỉ:** `b *0x08048456` (Dừng tại địa chỉ bộ nhớ cụ thể).
  * **Xem danh sách:** `info b` (Xem các bẫy đang đặt).
  * **Gỡ bẫy:** `del 1` (Xóa bẫy số 1).

### 2\. Watchpoints (Điểm dừng động) - *Hacker Pro dùng cái này*

Dừng lại khi **giá trị của một biến thay đổi**.

  * *Tình huống:* Bạn có biến `score`. Bạn muốn biết dòng code nào làm thay đổi điểm số?
  * **Lệnh:** `watch score` (Hoặc `watch *0x08049000`).
  * **Tác dụng:** Chương trình sẽ chạy vù vù, và tự động "phanh gấp" ngay khoảnh khắc giá trị tại địa chỉ đó thay đổi từ cũ sang mới.

-----

## 🔍 MODULE 5: ĐỌC STACK & MEMORY (SOI BỘ NHỚ)

Đây là kỹ năng quan trọng nhất để khai thác lỗi **Buffer Overflow**. Bạn cần nhìn thấy dữ liệu đang nằm trong RAM.

### Lệnh `x` (Examine - Kiểm tra)

Cú pháp: `x/[số_lượng][định_dạng][kích_thước] [địa_chỉ]`

  * **Ví dụ 1:** Xem 10 dòng đầu của Stack Pointer (`$rsp`).

    ```gdb
    x/10gx $rsp
    ```

      * `10`: Xem 10 đơn vị.
      * `g`: Giant (8 bytes - chuẩn 64bit).
      * `x`: Hex (Hệ thập lục phân).
      * `$rsp`: Địa chỉ bắt đầu xem.

  * **Ví dụ 2:** Xem chuỗi ký tự (String) tại địa chỉ `0x402010`.

    ```gdb
    x/s 0x402010
    ```

      * `s`: String (Hiển thị dạng chữ cái).

-----

## 🧪 BÀI TẬP LAB: GỠ LỖI CHƯƠNG TRÌNH CRACKME

**Mục tiêu:** Tìm mật khẩu ẩn trong chương trình mà không cần nhìn source code.

**Bước 1: Tạo chương trình C đơn giản (`crackme.c`)**

```c
#include <stdio.h>
#include <string.h>

int main() {
    int secret = 1337;
    int input;
    printf("Nhap mat khau so: ");
    scanf("%d", &input);
    if (input == secret) {
        printf("Correct!\n");
    } else {
        printf("Wrong!\n");
    }
    return 0;
}
```

Biên dịch: `gcc crackme.c -o crackme -g` (Thêm `-g` để debug dễ hơn).

**Bước 2: Nạp vào GDB**

```bash
gdb ./crackme
```

**Bước 3: Phân tích**

1.  Gõ `b main` để dừng tại đầu chương trình.
2.  Gõ `r` để chạy. GDB dừng tại `main`.
3.  Gõ `ni` (Next Instruction) liên tục để đi qua từng dòng lệnh assembly.
4.  **Quan sát:** Hãy nhìn vào cửa sổ **DISASM** của Pwndbg.
      * Bạn sẽ thấy một dòng lệnh so sánh: `cmp eax, 0x539`.
      * Hoặc `cmp [rbp-4], 0x539`.

**Bước 4: Giải mã**

  * Bạn thấy số `0x539`.
  * Mở Terminal khác đổi sang thập phân: `python3 -c "print(0x539)"` → Kết quả là `1337`.
  * → **Mật khẩu là 1337!**

-----

## ⚠️ PRO TIP: TẠI SAO PHẢI ĐỌC STACK?

Trong bài sau (Buffer Overflow), chúng ta sẽ nhồi quá nhiều chữ "A" vào một biến.
Dùng GDB, bạn sẽ thấy Stack tràn ngập `0x41414141` (`A` trong Hex là `41`).
Nếu bạn thấy `0x41414141` nằm chễm chệ ngay tại thanh ghi `RIP` → **Chúc mừng, bạn đã chiếm quyền điều khiển dòng lệnh của chương trình!**

-----

**Mission Completed!**
Bạn đã biết cách mổ xẻ chương trình đang chạy.
