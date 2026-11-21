# Phiếu QC 1 — Road v1.0

Lô được kiểm: **Kiều Quang Trường — 03205**.  
Người QC được chỉ định: **Nguyễn Minh Nam Hải — 2A202603076**.  
Ngày: **21/11/2026**. Guideline: **Road v1.0**.  
Bản kiểm: **revision 5**, theo [manifest](kieuquangtruong-manifest.json).  
File: [front](kieuquangtruong-front.zip), [fisheye](kieuquangtruong-fisheye.zip).


## Phạm vi

- Kiểm cấu trúc toàn bộ **28 ảnh**: 20 core, 8 extra.
- Bộ cuối có **265 shape + 112 tag = 377 nhãn duy nhất**. Không coi 377 là mẫu số của lỗi hình học khi mới kiểm cấu trúc.
- Kết quả máy: **13 cảnh báo = 2 nặng + 11 chính + 0 nhẹ**.
- Xác nhận riêng: coach đã chấp nhận ngoại lệ A05 không thấy vạch; học viên đã kiểm thực tế ba road_curb B02 và xác nhận các đoạn ngắn bám đúng mép đường.
- Output gốc giữ tại [lint-results.txt](lint-results.txt); không sửa checker hoặc log để làm mất cảnh báo.

## Tổng hợp theo mã

Số mục bên dưới mô tả đúng phép kiểm đã thực hiện; chưa kiểm tay không đồng nghĩa với 0 lỗi.

| Mã | Loại lỗi | Số mục kiểm | Kết quả / số lỗi xác nhận | Mức |
| --- | --- | --- | --- | --- |
| QC1-01 tag camera | sai thuộc tính / bỏ sót | 112 tag / 28 ảnh | Đủ bốn tag; camera theo tên ảnh; chưa phát hiện lỗi cấu trúc | nặng / nhẹ |
| QC1-02 đủ L1/R1 | bỏ sót | 16 ảnh front kiểm hiện diện position | A05 thiếu L1: ngoại lệ đã xác nhận; A09 thiếu R1: cần kiểm ảnh | nặng nếu thiếu thật |
| QC1-03 position | sai thuộc tính / thừa | 65 lane kiểm miền giá trị, quy tắc camera bên | Camera bên dùng other trong bộ gộp; ý nghĩa từng vị trí cần kiểm theo ảnh | nặng |
| QC1-04 tâm, chiều, điểm dừng | sai phạm vi / bỏ sót | 52 lane front kiểm chiều điểm | Không còn cảnh báo chiều; chưa chứng nhận sai số tâm và điểm dừng | chính |
| QC1-05 vạch đứt, che | sai phạm vi / thuộc tính | 65 lane kiểm position trùng, đọc kind của 6 nhóm cảnh báo | Năm nhóm đều double/double; A04 L1 là single/double. Không tự coi sáu nhóm là sáu lỗi tách vạch đứt | chính |
| QC1-06 style/color/kind | sai thuộc tính | 65 lane kiểm thuộc tính trong XML | Giá trị hợp lệ; A04 hai L1 khác kind cần đối chiếu ảnh | chính |
| QC1-07 vùng lái | sai phạm vi | 48 drivable + 30 crosswalk kiểm cấu trúc | Có đủ loại thuộc tính; biên vùng và direct/alternative chưa kiểm đầy đủ | chính |
| QC1-08 đèn, màu | bỏ sót / thuộc tính | 48 box đèn kiểm thuộc tính | Chưa kết luận màu đang sáng, độ sát hộp hoặc bỏ sót theo ảnh | nặng / chính |
| QC1-09 facing/type/group | sai thuộc tính | 86 box đèn và biển kiểm thuộc tính | Chưa kết luận đúng nghĩa theo ảnh; không phát hiện giá trị lạ trong lint | chính |
| QC1-10 đủ biển/hình in | thừa / bỏ sót | 38 box biển kiểm cấu trúc | Chưa kiểm đầy đủ mọi biển ngoài hộp hoặc hình in/phản chiếu | chính |
| QC1-11 fisheye | sai phạm vi / thuộc tính | 31 polyline fisheye kiểm số điểm; 3 road_curb B02 có xác nhận kiểm thực tế | Ba road_curb B02 giữ nguyên; hai lane B04/B08 còn cần kiểm cung | chính |
| QC1-12 file | lỗi file | 2 ZIP, 28 tên ảnh; đối chiếu manifest | ZIP đọc được, không kèm ảnh, đủ tag; chưa phát hiện lỗi file | nhẹ / nặng |

## Danh sách phát hiện và xử lý theo output mới

### QC1-02: hiện diện L1/R1

| Ảnh | Phát hiện | Xử lý |
| --- | --- | --- |
| A05 | Chưa có L1 trong XML | Coach đã xác nhận không quan sát được vạch; không thêm đường suy diễn. Export hiện có một R1, cần bảo đảm đường này là vạch sơn thật, không phải mép đường |
| A09 | Chưa có R1 trong XML | Cần rà ảnh: nếu thấy vạch phải gán đúng position; nếu thật sự không thấy cần xác nhận ngoại lệ riêng. Xác nhận A05 không áp dụng cho A09 |

### QC1-05: các nhóm position trùng

| Ảnh | Position | kind trong export | Nhận xét |
| --- | --- | --- | --- |
| A01 | L2 | double / double | Phù hợp giải thích hai nét của vạch đôi; cảnh báo trùng chưa chứng minh tách vạch đứt |
| A04 | L1 | single / double | Hai đường khác kind; cần kiểm và thống nhất theo đối tượng thật |
| A06 | L1 | double / double | Ghi nhận giải thích vạch đôi; cần người QC xác nhận cách biểu diễn theo hướng dẫn coach |
| A09 | L2 | double / double | Ghi nhận giải thích vạch đôi; kiểm đồng thời cảnh báo thiếu R1 |
| A11 | L1 | double / double | Ghi nhận giải thích vạch đôi; không đổi position tùy ý để hết cảnh báo |
| A14 | L1 | double / double | Ghi nhận giải thích vạch đôi; không tự kết luận lỗi tách vạch đứt |

Người học giải thích những đường cùng position là hai nét của vạch đôi. XML xác nhận năm nhóm đều có kind=double; riêng A04 có một single và một double. Chưa có xác nhận coach về cách biểu diễn bằng hai polyline nên phiếu không tự ghi toàn bộ các nhóm là đúng hoặc sai. Không suy diễn quy tắc “đặt giữa hai nét” nếu guideline chưa nêu rõ.

### QC1-11: số điểm trên fisheye

| Ảnh | Nhãn theo log / điểm đầu | Cảnh báo | Xử lý |
| --- | --- | --- | --- |
| B02 | road_curb#1 @ (1001,515) | 2 điểm | Học viên xác nhận đã kiểm thực tế, đoạn ngắn và bám mép đường; giữ nguyên |
| B02 | road_curb#12 @ (1190,659) | 2 điểm | Học viên xác nhận đã kiểm thực tế, đoạn ngắn và bám mép đường; giữ nguyên |
| B02 | road_curb#13 @ (1056,552) | 2 điểm | Học viên xác nhận đã kiểm thực tế, đoạn ngắn và bám mép đường; giữ nguyên |
| B04 | lane#5 @ (964,438) | 3 điểm | Chưa có xác nhận kiểm thực tế riêng cho lane; cần kiểm bám tâm vạch và cung |
| B08 | lane#0 @ (215,617) | 3 điểm | Chưa có xác nhận kiểm thực tế riêng cho lane; cần kiểm bám tâm vạch và cung |

Ít điểm chỉ là gợi ý của lint. Chỉ tính lỗi hình học khi đường thực sự lệch so với guideline; không thêm điểm thẳng hàng chỉ để mất cảnh báo.

## Ca mơ hồ

| Ca | Trạng thái |
| --- | --- |
| A05 không thấy vạch | Coach đã xác nhận ngoại lệ theo thông tin học viên |
| Ba đoạn road_curb B02 ngắn | Đã kiểm thực tế theo xác nhận học viên, giữ nguyên |
| Biểu diễn vạch đôi bằng hai polyline chung position | Đã ghi nhận giải thích; chờ xác nhận cách biểu diễn theo coach, chú ý A04 single/double |
| A09 chưa có R1 | Cần kiểm ảnh hoặc xác nhận ngoại lệ riêng |
| Lane B04/B08 có 3 điểm | Chưa có kết luận kiểm cung riêng |

## Quyết định

**Quyết định cuối của Nguyễn Minh Nam Hải: chờ xác nhận.**

Không dùng 13 cảnh báo làm 13 lỗi thật. A05 là ngoại lệ đã được coach chấp nhận; ba road_curb B02 đã được học viên xác nhận kiểm thực tế. Còn A09, A04 và các mục hình học chưa khép lại nên chưa đủ bằng chứng tính tỉ lệ lỗi chính hoặc ghi Release. Không tự ghi Hold/Rework thay người QC khi chưa có kết luận kiểm tay.

Sau khi Nam Hải xác nhận các mục còn mở, ghi số lỗi thật, số nhãn được kiểm theo tiêu chí tương ứng và quyết định theo Road v1.0: còn lỗi nặng hoặc lỗi chính >5% thì Rework; còn ca chờ lead thì Hold; đủ điều kiện thì Release.

Phiếu này là **Nam Hải kiểm bài Trường**, không thay phiếu **Trường kiểm bài bạn được ghép** trong bộ nộp.
