# BÀI LÀM PHA 2 – HACKATHON 02

- **Mã đề:** 08
- **Họ tên:Đặng Minh Huyền**
- **Lớp:CNTT1**
- **Link repository Pha2:https://github.com/huyenminhmeomeo-077/HN-KS26-CNTT1_PromptAI_H02_Pha1-Pha2**
- **Công cụ AI đã dùng (ChatGPT/Claude/Gemini):Gemini**

> Pha 2 (20 phút cuối). Mỗi mục: mở cuộc trò chuyện mới, gửi nguyên văn prompt đã viết trong bai_lam_pha1.md, dán câu trả lời và link chia sẻ vào đây. Xong, kéo thả file này vào repository Pha2.

## 4.2. Trích xuất tin nhắn đặt lịch khám ra JSON

**KET QUA AI**

```text
JSON
[
  {
    "ma_dich_vu": "TRAM",
    "ngay_hen": "2026-12-17",
    "tre_em": 1,
    "nguoi_lon": 2,
    "khung_gio": "chieu",
    "ghi_chu": null
  },
  {
    "ma_dich_vu": "CAOVOI",
    "ngay_hen": "2027-01-06",
    "tre_em": 0,
    "nguoi_lon": 3,
    "khung_gio": null,
    "ghi_chu": "1 người bị tiểu đường"
  },
  {
    "ma_dich_vu": "TAYTRANG",
    "ngay_hen": null,
    "tre_em": 0,
    "nguoi_lon": 4,
    "khung_gio": "sang",
    "ghi_chu": "1 người dị ứng thuốc tê"
  }
]
```

**Link trò chuyện:https://share.gemini.google/RD8CeSAixu8g**

## 4.3. Báo giá bằng Chain-of-Thought

**KET QUA AI**

```text
Python
caovoi_adult = 350_000
num_adults = 6
num_kids_6_12 = 2 # 7 tuổi và 11 tuổi
kid_rate = 0.80
num_kids_under_6 = 1 # 4 tuổi

adult_service = num_adults * caovoi_adult
kid_6_12_service = num_kids_6_12 * (caovoi_adult * kid_rate)
kid_under_6_service = 0

total_service_before_discount = adult_service + kid_6_12_service + kid_under_6_service
# Nhóm từ 5 người lớn trở lên (6 >= 5) -> giảm 10% trên tiền dịch vụ (tiền người lớn + tiền trẻ em)
discount_rate = 0.10
discount_amount = int(total_service_before_discount * discount_rate)
service_after_discount = total_service_before_discount - discount_amount

xray_price = 150_000
num_xray = 3
xray_total = num_xray * xray_price

grand_total = service_after_discount + xray_total

print(f"Adult service: {adult_service:,}")
print(f"Kids 6-12 service: {kid_6_12_service:,.0f}")
print(f"Total service before discount: {total_service_before_discount:,.0f}")
print(f"Discount (10%): {discount_amount:,.0f}")
print(f"Service after discount: {service_after_discount:,.0f}")
print(f"X-ray total: {xray_total:,}")
print(f"Grand total: {grand_total:,.0f}")

Kết quả chạy mã
Adult service: 2,100,000
Kids 6-12 service: 560,000
Total service before discount: 2,660,000
Discount (10%): 266,000
Service after discount: 2,394,000
X-ray total: 450,000
Grand total: 2,844,000

Tổng chi phí báo giá cho nhóm là 2.844.000 đồng.
Chi tiết cách tính
Hạng mục	Cách tính	Thành tiền (đồng)
Dịch vụ CAOVOI (6 người lớn)	$6 \times 350.000$	2.100.000
Dịch vụ CAOVOI (2 trẻ 7 và 11 tuổi)	$2 \times (350.000 \times 80\%)$ = $2 \times 280.000$	560.000
Bé 4 tuổi (dưới 6 tuổi)	Miễn phí	0
Tổng tiền dịch vụ trước giảm	$2.100.000 + 560.000$	2.660.000
Giảm giá nhóm (6 người lớn $\ge 5$)	$-10\% \times 2.660.000$	-266.000
Tiền dịch vụ sau giảm	$2.660.000 - 266.000$	2.394.000
Phí chụp X-quang (3 phim, không giảm)	$3 \times 150.000$	450.000
Tổng cộng	$2.394.000 + 450.000$	2.844.000
```

**Link trò chuyện:https://share.gemini.google/X80ldOdPM07K**

### b) Kiểm tra kết quả AI
Đúng
## 4.4. Dây chuyền xử lý khiếu nại – kết quả Trạm 1

**KET QUA AI**

```text
Chào bạn, có vẻ như bạn đang thiết kế một CREATE prompt (Context, Request, Explanation, Action, Tone, Extras) cho hệ thống xử lý khiếu nại tự động 3 trạm của Nha khoa Nụ Cười, nhưng nội dung các trạm đang bị ghép chung vào một khung và còn thiếu dữ liệu đầu vào (nội dung email khách hàng, chính sách đền bù cụ thể).

Để hệ thống chạy chính xác theo kiến trúc 3 trạm (mỗi trạm chỉ làm đúng một nhiệm vụ chuyên biệt), mình đã tách và chuẩn hóa lại bộ prompt cho từng trạm dưới đây. Bạn chỉ cần điền dữ liệu thực tế vào các thẻ XML khi vận hành:

Trạm 1: Phân loại khiếu nại & Trích xuất dữ liệu
(Chỉ nhận email gốc và trả về JSON, không tính tiền hay viết thư)

Plaintext
C - Context: Bạn là Trạm 1 trong dây chuyền xử lý khiếu nại 3 trạm của Nha khoa Nụ Cười. Nhiệm vụ duy nhất của bạn là đọc email khiếu nại, phân loại sự cố và trích xuất số liệu khách quan.
R - Request: Đọc email trong thẻ <email_khach_hang> và trả về duy nhất một đối tượng JSON hợp lệ gồm đúng 4 trường:
{
  "loai_su_co": "tre_hen" | "thiet_bi" | "bac_si" | "khac",
  "so_khach": số nguyên,
  "so_lan_hen_bi_tre": số nguyên hoặc null,
  "tom_tat": "tóm tắt nội dung khiếu nại, không quá 20 từ"
}
E - Explanation: 
- "tre_hen": khách phàn nàn về việc chờ đợi lâu, trễ giờ hẹn, dời lịch đột ngột.
- "thiet_bi": phàn nàn về máy móc, cơ sở vật chất, vệ sinh dụng cụ.
- "bac_si": phàn nàn về thái độ, tay nghề của bác sĩ hoặc nhân viên y tế.
- "khac": các vấn đề còn lại (chi phí, hóa đơn, tư vấn...).
- Nếu email không đề cập số lần bị trễ hẹn, đặt "so_lan_hen_bi_tre" là null.
A - Action:
1) Đọc kỹ nội dung trong thẻ <email_khach_hang>.
2) Xác định loại sự cố theo đúng 1 trong 4 nhãn quy định.
3) Đếm chính xác số lượng khách bị ảnh hưởng và số lần hẹn bị trễ/chậm như email nêu.
4) Viết tóm tắt ngắn gọn dưới 20 từ.
5) Xuất ra duy nhất khối JSON, không kèm lời giải thích hay ký tự thừa.
T - Tone: Trung lập, chính xác, khách quan.
E - Extras: 
<email_khach_hang>
[Dán nội dung email khiếu nại của khách hàng vào đây]
</email_khach_hang>
Trạm 2: Tính toán mức đền bù & Quyền phê duyệt
(Nhận JSON từ Trạm 1 + Chính sách đền bù để ra quyết định tài chính)

Plaintext
C - Context: Bạn là Trạm 2 (Bộ phận Xử lý Chính sách & Đền bù) trong dây chuyền xử lý khiếu nại của Nha khoa Nụ Cười.
R - Request: Đối chiếu dữ liệu từ <ket_qua_tram_1> với quy định trong <chinh_sach>, trả về duy nhất một đối tượng JSON gồm đúng 3 trường:
{
  "loai_su_co": "giá trị lấy từ Trạm 1",
  "muc_den_bu": tổng số tiền đền bù bằng đồng (số nguyên) hoặc null,
  "can_duyet": true nếu muc_den_bu lớn hơn 500000, ngược lại là false
}
E - Explanation: 
- Tính tổng số tiền dựa trên số khách và số lần trễ hẹn (nếu chính sách quy định tính theo lượt/người).
- Nếu sự cố không thuộc diện đền bù tiền mặt hoặc mức đền bù là 0, trả về null cho "muc_den_bu" và false cho "can_duyet".
A - Action:
1) Đọc kết quả phân tích từ <ket_qua_tram_1>.
2) Áp dụng đúng công thức/định mức trong <chinh_sach>.
3) Kiểm tra ngưỡng 500.000đ để gắn cờ "can_duyet".
4) Xuất ra duy nhất đối tượng JSON.
T - Tone: Chính xác, tuân thủ quy tắc.
E - Extras:
<ket_qua_tram_1>
[Dán output JSON của Trạm 1 vào đây]
</ket_qua_tram_1>

<chinh_sach>
[Dán bảng quy định mức đền bù của nha khoa vào đây, ví dụ: Trễ hẹn đền bù 100.000đ/lần/khách...]
</chinh_sach>
Trạm 3: Soạn email phản hồi khách hàng
(Nhận JSON từ Trạm 2 và viết thư xin lỗi gửi khách)

Plaintext
C - Context: Bạn là nhân viên Chăm sóc khách hàng của Nha khoa Nụ Cười (Trạm 3 - Phản hồi khách hàng).
R - Request: Dựa vào dữ liệu trong thẻ <ket_qua_tram_2>, viết một email phản hồi hoàn chỉnh gửi khách hàng (gồm Tiêu đề và Nội dung) với độ dài dưới 150 từ.
E - Explanation: Email cần thể hiện sự cầu thị, xin lỗi về trải nghiệm chưa tốt và thông báo phương án hỗ trợ/đền bù chính xác theo dữ liệu được cấp.
A - Action:
1) Đọc thông tin trong <ket_qua_tram_2>.
2) Gửi lời xin lỗi chân thành về sự cố gặp phải.
3) Nêu chính xác số tiền trong "muc_den_bu" (nếu là null thì không đề cập đền bù tiền mặt, chỉ cam kết khắc phục chất lượng dịch vụ). Tuyệt đối không tự ý thay đổi hoặc bịa thêm mức đền bù khác.
4) Giữ toàn bộ phần nội dung email ngắn gọn dưới 150 từ.
T - Tone: Chân thành, lịch sự, chuyên nghiệp và đồng cảm.
E - Extras:
<ket_qua_tram_2>
[Dán output JSON của Trạm 2 vào đây]
</ket_qua_tram_2>
Nếu bạn muốn chạy thử nghiệm ngay dây chuyền này (ví dụ: bạn đang làm bài tập thiết kế prompt chuỗi/workflow), hãy gửi cho mình nội dung một email khiếu nại mẫu và quy định mức đền bù của nha khoa, mình sẽ chạy lần lượt qua cả 3 trạm để bạn kiểm tra kết quả!
```

**Link trò chuyện:https://share.gemini.google/dikN7C3QLPOL**

## 4.5. Prompt chấm bài theo Rubric

**KET QUA AI**

```text
Tiêu chí	Điểm	Minh chứng	Đề xuất sửa
Đúng thông tin (50%)	0,5 / 5,0	- Sai giá dịch vụ: Ghi "Trám răng bên em 450k/răng thôi" (thông tin chuẩn là 500.000 đồng/răng).

- Sai chính sách bảo hành: Ghi "Bong sau 8 tháng thì em làm lại free" (thông tin chuẩn: tháng 7 đến tháng 12 giảm 50%, chỉ miễn phí trong 6 tháng đầu).
	- Sửa giá: "Dịch vụ trám răng tại nha khoa có chi phí 500.000 đồng/răng."

- Sửa bảo hành: "Nếu miếng trám bong trong 6 tháng đầu, nha khoa hỗ trợ làm lại miễn phí; từ tháng thứ 7 đến tháng thứ 12 sẽ được giảm 50%; sau 12 tháng sẽ không áp dụng bảo hành."

Đầy đủ (30%)	0,5 / 3,0	- Thiếu nội dung trả lời đầy đủ cho cả hai câu hỏi của khách: Bài tư vấn chỉ đề cập đến mức giá và một mốc thời gian bảo hành, không thể hiện việc giải đáp đầy đủ và chính xác cả hai nội dung hỏi của khách hàng.

- Thiếu lời mời đặt lịch: Không có câu mời khách cung cấp ngày hẹn và khung giờ để đặt lịch.	- Bổ sung đầy đủ thông tin và lời mời đặt lịch: Trình bày rõ ràng cả giá dịch vụ và toàn bộ chính sách bảo hành, sau đó thêm: "Anh/chị vui lòng cho biết ngày hẹn và khung giờ mong muốn để nha khoa hỗ trợ đặt lịch thăm khám."
Giọng điệu (20%)	0,5 / 2,0	- Dùng từ ngữ suồng sã, thiếu chuyên nghiệp: Sử dụng các từ/cụm từ "bên em", "thôi", "free", "nha", "cứ inbox em ạ!", kèm lời quảng cáo chủ quan "đẹp như răng thật luôn".	- Dùng từ ngữ chuẩn mực, lịch sự: Thay bằng "nha khoa", "miễn phí", "ạ", "liên hệ với nha khoa"; diễn đạt khách quan, chuyên nghiệp thay cho lời khẳng định suồng sã.
Tổng điểm	1,5 / 10	—	—
```

**Link trò chuyện:https://share.gemini.google/XjQnj4Hygvtb**