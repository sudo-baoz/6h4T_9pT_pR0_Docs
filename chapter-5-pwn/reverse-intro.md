# 🧩 Mission 23: Dịch Ngược (Reverse Engineering) Là Gì?

> **📜 Mission Brief:** Các phần mềm thương mại hoặc mã độc (Malware) không bao giờ đưa cho bạn mã nguồn (Source Code). Chúng chỉ đưa cho bạn file chạy (`.exe`, `.elf`).
>
> **Reverse Engineering** là quá trình phân tích file chạy đó để hiểu logic hoạt động bên trong, tìm thuật toán sinh key, tìm lỗ hổng bảo mật hoặc bẻ khóa phần mềm.

![Image of compilation and reverse engineering process flow chart](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQNWVNS3nRaVE85UkAWuPyj_GqJDuu0tUwmVg&s)

-----

## 🏗️ MODULE 1: QUÁ TRÌNH BIÊN DỊCH & DỊCH NGƯỢC

Để hiểu RE, bạn phải hiểu máy tính chạy code như thế nào.

### 1\. Chiều xuôi (Compilation)

Khi bạn viết code C/C++:

1.  **Source Code (`.c`):** Ngôn ngữ con người hiểu (Tiếng Anh, Logic).
2.  **Compiler (GCC/Clang):** Dịch sang ngôn ngữ máy.
3.  **Machine Code (Binary):** Chuỗi `010101` hoặc Hex `4A 8B 00`. Chỉ CPU hiểu.

### 2\. Chiều ngược (Reversing)

Vì quá trình biên dịch làm mất rất nhiều thông tin (tên biến, comment), chúng ta không thể khôi phục 100% về source code gốc. Chúng ta có 2 cấp độ dịch ngược:

  * **Disassembly (Giải mã hợp ngữ):** Dịch mã máy (`55 89 E5`) sang **Assembly** (`PUSH EBP; MOV EBP, ESP`). Đây là bản dịch chính xác 1-1, nhưng cực kỳ khó đọc với người thường.
  * **Decompilation (Dịch ngược giả mã):** Cố gắng tái tạo lại cấu trúc giống **C/C++** (Pseudo-code). Dễ đọc hơn, nhưng đôi khi không chính xác hoàn toàn.

-----

## 🛠️ MODULE 2: KHO VŨ KHÍ REVERSE (THE ARSENAL)

Trong RE, công cụ là tất cả. Dưới đây là những cái tên "thống trị" thế giới CTF và Malware Analysis.

### 1\. IDA Pro (The Gold Standard)

  * **Vị thế:** Là công cụ mạnh nhất, đắt đỏ nhất và phổ biến nhất trong giới chuyên nghiệp.
  * **Tính năng:** Disassembler cực xịn, vẽ biểu đồ luồng (Graph View) tuyệt đẹp. Bản Free (IDA Free) đủ dùng cho người mới bắt đầu.
  * **Điểm mạnh:** Plugin hệ sinh thái khổng lồ.

### 2\. Ghidra (The NSA Tool) 🌟 *Khuyên dùng cho sinh viên*

  * **Xuất thân:** Được Cơ quan An ninh Quốc gia Hoa Kỳ (NSA) phát triển và open-source năm 2019.
  * **Đặc điểm:** Hoàn toàn **MIỄN PHÍ**. Có tính năng **Decompiler** (biến Assembly thành C) cực mạnh mà trước đây chỉ IDA bản trả phí ngàn đô mới có.
  * **Giao diện:** Viết bằng Java, hơi nặng nhưng rất nhiều tính năng.

### 3\. Debuggers (x64dbg / GDB)

  * Nếu IDA/Ghidra dùng để **nhìn** (Tĩnh - Static Analysis), thì Debugger dùng để **chạy** (Động - Dynamic Analysis).
  * Bạn dùng nó để cho chương trình chạy từng bước một, xem giá trị trong thanh ghi (Register) và RAM thay đổi thế nào.

-----

## 🧠 MODULE 3: TƯ DUY DỊCH NGƯỢC (MINDSET)

Làm sao để hiểu một đống mã Assembly hỗn độn?

### Ví dụ thực tế: Hàm kiểm tra mật khẩu

Giả sử chương trình gốc (C++) có đoạn code:

```cpp
if (input == 1234) {
    print("Correct");
} else {
    print("Wrong");
}
```

Khi nhìn vào **Disassembler (Assembly)**, bạn sẽ thấy:

```asm
MOV EAX, [ebp-4]    ; Lấy giá trị người dùng nhập đưa vào thanh ghi EAX
CMP EAX, 0x4D2      ; So sánh EAX với 1234 (0x4D2 là số Hex của 1234)
JZ 0x401020         ; JZ = Jump if Zero (Nếu bằng nhau thì nhảy tới địa chỉ Correct)
PUSH "Wrong"        ; Nếu không bằng, chuẩn bị in chữ Wrong
CALL printf         ; Gọi hàm in
JMP 0x401030        ; Kết thúc
```

**Nhiệm vụ của Reverser:**

1.  Tìm lệnh `CMP` (So sánh).
2.  Thấy nó so sánh với `0x4D2`.
3.  Dùng máy tính đổi `0x4D2` sang thập phân → `1234`.
4.  → **Tìm ra mật khẩu!**

-----

## 🧪 BÀI TẬP LAB: CÀI ĐẶT GHIDRA

Vì bạn là sinh viên và thích mã nguồn mở, Ghidra là lựa chọn số 1.

1.  **Yêu cầu:** Máy đã cài Java (JDK 11+).
2.  **Tải về:** Vào trang chủ `ghidra-sre.org` tải file zip.
3.  **Cài đặt:** Giải nén và chạy file `./ghidraRun` (Linux) hoặc `ghidraRun.bat` (Windows).
4.  **Thử nghiệm:**
      * Viết một chương trình C đơn giản "Hello World".
      * Compile nó ra file `.exe` (hoặc ELF).
      * Kéo file đó vào Ghidra.
      * Bấm **Analyze** và nhìn vào cửa sổ **Decompile** xem nó có hiện lại code C ban đầu không.

-----

## ⚠️ LƯU Ý PHÁP LÝ

> **Reverse Engineering là con dao hai lưỡi.**
>
>   * **Hợp pháp:** Phân tích mã độc để phòng chống, nghiên cứu học tập, tham gia CTF, Bug Bounty.
>   * **Bất hợp pháp:** Bẻ khóa phần mềm bản quyền (Crack), tạo cheat game online, sao chép mã nguồn thương mại.
>
> *Là một chuyên gia bảo mật tương lai, hãy luôn đứng về phía "Mũ Trắng" (White Hat).*

-----

**Mission Completed\!**
Bạn đã nắm được khái niệm cốt lõi của việc "đọc code máy".
