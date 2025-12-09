# 🛡️ MISSION 3: ĐẠO ĐỨC HACKER & LUẬT CHƠI

<p align="center">
	<img src="https://img.shields.io/badge/Ethics-Professional-blue?style=for-the-badge" alt="ethics" />
	<img src="https://img.shields.io/badge/Legal-Read%20First-red?style=for-the-badge" alt="legal" />
</p>

> *"With great power comes great responsibility."* — Sức mạnh càng lớn, trách nhiệm càng cao.

---

## 🎯 Tóm tắt nhanh

- Học kỹ thuật để xây dựng phòng thủ, không để hại người khác.
- Luôn có **giấy phép** trước khi thử nghiệm trên hệ thống nào đó.
- Trong CTF chỉ tấn công vào challenge; tránh DoS, tránh chia sẻ flag.

---

## 🎩 1. Bạn đội mũ màu gì? (Quick reminder)

| Loại Hacker | Biểu tượng | Mục tiêu | Tính hợp pháp |
|---|:---:|---|---|
| **White Hat** | ⚪ | Tìm và vá lỗ hổng | Hợp pháp (được ủy quyền) |
| **Grey Hat** | 🔘 | Thử nghiệm không xin phép nhưng báo cáo | Rủi ro; dễ gặp rắc rối pháp lý |
| **Black Hat** | ⚫ | Xâm nhập để trục lợi | Bất hợp pháp (có thể bị truy tố) |

> Chúng ta hướng tới **White Hat**: tư duy kẻ tấn công để bảo vệ, luôn có permission.

---

## 📜 2. Nguyên tắc vàng (The Golden Rules)

1. **Permission — luôn có bằng văn bản.**
2. **Scope — chỉ làm trong phạm vi được cho phép.**
3. **Privacy — không lấy, không sao chép dữ liệu nhạy cảm; báo cáo đúng cách.**

Ngắn gọn: nếu không chắc, dừng lại và hỏi.

---

## ⚖️ 3. Khung pháp lý (Vietnam) — những hành vi dễ gặp rắc rối

- Truy cập trái phép, làm rối loạn dịch vụ (DoS), chiếm đoạt dữ liệu, phát tán mã độc đều có thể bị xử lý theo Bộ luật Hình sự và Luật An ninh mạng.
- Luôn kiểm tra luật địa phương trước khi làm thực nghiệm ngoài lab.

Tham khảo: [Luật An ninh mạng](https://vanban.chinhphu.vn/?pageid=27160&docid=206114) (tham khảo chính thức từ các nguồn nhà nước).

---

## 🚩 4. CTF Etiquette — quy tắc khi thi

### 🚫 Không nên làm (Don'ts)

1. Không tấn công hạ tầng của Ban Tổ Chức (scoreboard, DB, infra).
2. Không làm DoS bài thi hoặc gây gián đoạn cho người khác.
3. Không chia sẻ flag trước khi cuộc thi kết thúc.
4. Không công bố exploit/chi tiết nhạy cảm khi chưa có phép.

### ✅ Nên làm (Dos)

1. Báo cáo lỗi hạ tầng cho BTC nếu phát hiện (private message).
2. Tôn trọng đội khác; chơi fair.
3. Viết write-up sau trận (public only khi event cho phép).

---

## ✅ 5. Checklist an toàn (copy-paste)

- [ ] Luôn thử nghiệm trong **VM hoặc lab** bạn sở hữu.
- [ ] Có **quyền rõ ràng** bằng văn bản trước khi quét/hacking hệ thống bên ngoài.
- [ ] Ghi log, chụp màn hình, và dùng kênh an toàn để báo cáo phát hiện.
- [ ] Không download/giữ dữ liệu nhạy cảm; báo cáo và xóa nếu được phép.

---

## 🧾 6. Responsible Disclosure — Mẫu báo cáo (ẩn trong details)

<details>
<summary>Template: Báo cáo lỗ hổng (nhấn để mở)</summary>

Tiêu đề: [Vulnerabilitiy] <short description>

Kính gửi [Tên tổ chức / Admin],

Tôi là [Tên bạn], researcher/CTFer. Trong quá trình test trên [target], tôi phát hiện lỗ hổng: **<mô tả ngắn>**.

Chi tiết kỹ thuật:
- Thời gian phát hiện: YYYY-MM-DD
- Môi trường: [production/staging/test]
- Mô tả: <bước tái tạo ngắn gọn>
- Ảnh / log: (đính kèm)

Tác động: <confidentiality/integrity/availability impact>

Gợi ý khắc phục: <short mitigation>

Xin vui lòng xác nhận đã nhận được báo cáo này và cho biết kênh liên hệ để trao đổi thêm. Tôi sẵn sàng phối hợp để khắc phục.

Trân trọng,
[Tên bạn] — [email / pgp-key]

</details>

---

## ✉️ 7. Mẫu xin phép thử nghiệm (Sample permission email)

<details>
<summary>Mẫu xin phép (nhấn để mở)</summary>

Tiêu đề: Yêu cầu phép thử nghiệm bảo mật cho [tên dịch vụ]

Kính gửi [Tổ chức],

Tôi là [Tên], đại diện cho [tên nhóm/khóa học]. Chúng tôi mong muốn thực hiện một bài kiểm thử bảo mật nhỏ nhằm mục đích học tập/đánh giá trên [target] trong khoảng thời gian [dd-mm-yyyy].

Phạm vi: [liệt kê host, domain, IP]
Thời gian đề xuất: [start — end]
Cách thức: non-destructive testing, không DoS, không tải dữ liệu.

Xin cho biết nếu cần giấy tờ hoặc hợp đồng NDA. Cảm ơn và mong nhận phản hồi.

Trân trọng,
[Tên] — [email] — [số điện thoại]

</details>

---

## 📚 8. Nơi luyện tập hợp pháp

- Hack The Box, TryHackMe, PortSwigger Academy, CTFtime.
- Bug bounty: HackerOne, Bugcrowd, Intigriti; Việt Nam: VNCERT/CC.

---

## 📝 KẾT LUẬN

Đạo đức không phải là một dòng chữ ở cuối slide — nó là quy tắc sống còn để bạn tồn tại lâu dài trong ngành. Hãy luôn làm việc có trách nhiệm.

> **Next:** Mission 4 — Linux command line (Bash).