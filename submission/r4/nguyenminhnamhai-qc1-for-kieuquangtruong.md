# Phiếu QC 1 — Road v1.0

Lô được kiểm: **Kiều Quang Trường — 03205**.  
Người QC được chỉ định: **Nguyễn Minh Nam Hải — 2A202603076**.  
Ngày: **21/11/2026**. Phiên bản guideline: **Road v1.0**.  
Bản được kiểm: **revision 4** trong manifest.

**Nguồn lập phiếu:** Codex hỗ trợ kiểm trực tiếp các ZIP tại workspace và soạn theo đề nghị của học viên. Phiếu chưa có xác nhận cuối của Nguyễn Minh Nam Hải; không ghi như Hải đã tự kiểm hoặc ký. Không thay kiểm trực quan bằng kiểm schema.

## Phạm vi và bằng chứng

- Kiểm cấu trúc toàn bộ **28 ảnh**: A01–A16, B01–B12; gồm 20 ảnh core và 8 ảnh extra.
- **266 shape + 112 tag = 378 nhãn duy nhất** được kiểm ở mức cấu trúc, tên label và thuộc tính.
- Đối chiếu trực quan ảnh gốc A05; các ảnh khác chưa được kiểm hình học đầy đủ trong lượt lập phiếu này.
- Hai ZIP chỉ chứa annotations.xml, mở được; SHA-256 khớp manifest; 28 tên ảnh không trùng; mỗi ảnh có đúng một tag camera, timeofday, weather, source.
- Không phát hiện vi phạm schema thuộc tính so với configs/road_v1_labels.json.
- Chi tiết cảnh báo: [lint-results.txt](lint-results.txt). Hash và số lượng: [manifest](kieuquangtruong-manifest.json).

## Tổng hợp theo mã

“Số mục kiểm” dưới đây chỉ rõ phép kiểm đã thực hiện. “Chờ kiểm tay” không được hiểu là 0 lỗi. Các số mục giữa các mã có thể trùng đối tượng, không cộng thành số nhãn duy nhất.

| Mã | Loại lỗi | Số mục kiểm thực hiện | Số lỗi / kết quả | Mức |
| --- | --- | --- | --- | --- |
| QC1-01 tag camera | sai thuộc tính / bỏ sót | 112 tag trên 28 ảnh; camera theo hậu tố tên ảnh | Không phát hiện thiếu tag hoặc sai camera trong bộ cuối | nặng / nhẹ |
| QC1-02 đủ L1, R1 | bỏ sót | 16 ảnh front kiểm hiện diện position; 1 ảnh A05 đối chiếu ảnh gốc | Hai cảnh báo A05 là ngoại lệ không thấy vạch đã được coach xác nhận theo thông tin học viên | nặng nếu thiếu thật |
| QC1-03 position | sai thuộc tính / thừa | 67 lane kiểm giá trị hợp lệ, quy tắc other cho camera bên | Không phát hiện sai schema hoặc quy tắc camera bên; ngữ nghĩa từng vạch còn cần kiểm ảnh | nặng |
| QC1-04 tâm vạch, chiều, điểm dừng | sai phạm vi / bỏ sót | 54 lane front kiểm chiều theo thứ tự điểm; chưa đo lệch tâm hoặc điểm dừng | Không còn cảnh báo chiều trong bộ cuối; độ lệch 5 px chưa kết luận | chính |
| QC1-05 vạch đứt, che | sai phạm vi / sai thuộc tính | 67 lane kiểm position trùng; chưa xác minh occluded theo ảnh | 9 nhóm cảnh báo position trùng, chưa xác nhận là 9 lỗi thật | chính |
| QC1-06 style/color/kind | sai thuộc tính | 67 lane kiểm đủ thuộc tính và miền giá trị | Không vi phạm schema; đúng màu/style/kind theo pixel chưa kết luận | chính |
| QC1-07 vùng lái | sai phạm vi | 48 drivable + 28 crosswalk kiểm loại hình và thuộc tính; chưa tính IoU | Chờ kiểm biên, direct/alternative và thân xe | chính |
| QC1-08 đèn, màu | bỏ sót / sai thuộc tính | 48 box traffic_light kiểm thuộc tính | Không vi phạm schema; màu đang sáng và bỏ sót đèn chưa kết luận | nặng / chính |
| QC1-09 facing/type/group | sai thuộc tính | 86 box đèn/biển kiểm thuộc tính | Không vi phạm schema; ý nghĩa theo ảnh còn cần kiểm tay | chính |
| QC1-10 hình in, đủ biển | thừa / bỏ sót | 38 box traffic_sign kiểm cấu trúc; chưa rà đủ đối tượng trong ảnh | Chưa kết luận độ đầy đủ hoặc hình in/phản chiếu | chính |
| QC1-11 fisheye | sai phạm vi / thuộc tính | 32 polyline fisheye kiểm số điểm; 11 box kiểm edge_zone theo bán kính checker | 8 polyline ít điểm cần kiểm cung; không còn cảnh báo edge_zone | chính |
| QC1-12 file | lỗi file | 2 ZIP, 28 ảnh, 378 nhãn kiểm cấu trúc/schema và hash | Không phát hiện lỗi file hoặc schema | nhẹ / nặng |

## Các phát hiện cần rà tay

Các mục sau là cảnh báo máy cần kiểm, chưa phải danh sách lỗi hình học đã xác nhận.

| # | Mã | Ảnh | Nhãn / vị trí | Mức nếu xác nhận sai | Nội dung |
| --- | --- | --- | --- | --- | --- |
| 1 | QC1-05 | A01 | lane R2 | chính | Hai polyline cùng position |
| 2 | QC1-05 | A03 | lane L1 | chính | Hai polyline cùng position |
| 3 | QC1-05 | A04 | lane R2 | chính | Hai polyline cùng position |
| 4 | QC1-05 | A04 | lane L1 | chính | Hai polyline cùng position |
| 5 | QC1-05 | A06 | lane L1 | chính | Hai polyline cùng position |
| 6 | QC1-05 | A08 | lane R1 | chính | Ba polyline cùng position |
| 7 | QC1-05 | A09 | lane L2 | chính | Hai polyline cùng position |
| 8 | QC1-05 | A11 | lane L1 | chính | Hai polyline cùng position |
| 9 | QC1-05 | A14 | lane L1 | chính | Hai polyline cùng position |
| 10 | QC1-11 | B02 | road_curb#2, đầu (1190,659) | chính | Chỉ hai điểm |
| 11 | QC1-11 | B02 | road_curb#4, đầu (1056,552) | chính | Chỉ hai điểm |
| 12 | QC1-11 | B02 | road_curb#6, đầu (1001,515) | chính | Chỉ hai điểm |
| 13 | QC1-11 | B02 | road_curb#8, đầu (458,424) | chính | Chỉ hai điểm |
| 14 | QC1-11 | B02 | road_curb#9, đầu (529,387) | chính | Chỉ hai điểm |
| 15 | QC1-11 | B02 | road_curb#10, đầu (423,397) | chính | Chỉ ba điểm |
| 16 | QC1-11 | B04 | lane#5, đầu (964,438) | chính | Chỉ ba điểm |
| 17 | QC1-11 | B08 | lane#3, đầu (215,617) | chính | Chỉ ba điểm |

## Ca mơ hồ và xác nhận

| Ảnh | Nội dung | Trạng thái |
| --- | --- | --- |
| A05 | Không quan sát được vạch sơn phân làn; không tạo L1/R1 từ mép đường hoặc xe đỗ | Coach đã xác nhận ngoại lệ theo thông tin học viên. Hai cảnh báo máy giữ nguyên trong manifest, không tính là lỗi bỏ sót sau xác nhận |
| A01/A03/A04/A06/A08/A09/A11/A14 | Position lặp có phải đường tách hoặc gán sai position? | Cần người QC nhìn ảnh và đối chiếu từng đường |
| B02/B04/B08 | Đoạn ít điểm có bám cung trong sai số 5 px không? | Cần kiểm ảnh; không chèn điểm nội suy để làm mất cảnh báo |
| B06/B08/B12 | Các vùng drivable chỉ mang alternative | Cần kiểm chức năng vùng trực tiếp trong ảnh |

## Quyết định

- **Kết luận cấu trúc:** 28 ảnh, 378 nhãn; đủ tag, không vi phạm schema hoặc hash.
- **Cảnh báo máy:** 2 nặng tại A05 (ngoại lệ đã được coach xác nhận), 17 chính chưa xác nhận hình học, 0 nhẹ.
- **Lỗi nặng/chính/nhẹ sau kiểm tay:** chưa có thống kê đầy đủ. Không dùng 17/378 làm tỉ lệ lỗi chính vì 17 là số cảnh báo chưa xác nhận và 378 chỉ được kiểm cấu trúc.
- **Quyết định chính thức của Nguyễn Minh Nam Hải:** chờ Hải xác nhận sau khi rà hình học.
- **Đề xuất xử lý:** chưa Release; hoàn tất kiểm trực quan các mục còn mở rồi áp dụng ngưỡng 5% theo guideline. Đây là kiểm chưa hoàn tất, không tự gán Hold hoặc Rework thay người QC.

Hải có thể xác nhận nội dung kiểm, bổ sung các lỗi thật đã thấy, số nhãn kiểm tay và quyết định cuối. Phiếu này là phiếu Hải kiểm bài Trường; không thay phiếu Trường kiểm bài của bạn được ghép mà lab yêu cầu nộp.
