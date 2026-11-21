# Báo cáo cá nhân — Ngày 11 · Road Elements & SVM

Họ tên: **Kiều Quang Trường**. Mã học viên: **03205** (theo tên thư mục bài làm).  
Guideline: **Road v1.0**. Lần đóng gói: **revision 5**, ngày **21/11/2026**.  
Phiên bản CVAT: chưa có ảnh Help → About để xác nhận.  
Thời điểm export mới nhất: front A01–A12 **12:27:32**, front-extra A13–A16 **11:22:59**, ngày 21/11/2026 (UTC+7). Fisheye B01–B08 dùng fisheye_4.zip mới, export 12:34:50 UTC+7; extra B09–B12 dùng fisheye_extra.zip hiện có.

## 1. Kiểm project

| Mục | Kết quả kiểm | Ghi chú |
| --- | --- | --- |
| 6 label, đúng kiểu | Đúng khai báo metadata các export nguồn | lane, road_curb: polyline; crosswalk, drivable: polygon; traffic_light, traffic_sign: rectangle |
| Thuộc tính và giá trị | Không có cảnh báo tên label hoặc giá trị lạ | Các cảnh báo position trùng vẫn cần kiểm ảnh |
| 4 tag ảnh | Đủ trên 28/28 ảnh trong bộ đóng gói | camera, timeofday, weather, source; mỗi tag có thuộc tính value |

Cách đặt tag: chọn label kiểu Tag và đặt thuộc tính value, sau đó lưu. Cách kết thúc polyline trên bản CVAT đang dùng cần đối chiếu thao tác thực tế; báo cáo không suy đoán phím tắt hoặc phiên bản phần mềm.

Các ZIP gốc được giữ nguyên. Bộ gộp đã chuẩn hóa camera, position camera hai bên, edge_zone và chiều polyline như ghi ở mục 4. Các sửa cục bộ chưa được xác minh đồng bộ lại trên CVAT.

## 2. Số lượng đã gán

Số liệu đếm từ export cuối, không lấy từ bản nháp ban đầu. Tag không tính vào số shape.

| Task | Ảnh | lane | road_curb | crosswalk | drivable | traffic_light | traffic_sign |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| front | 12 | 38 | 14 | 8 | 30 | 30 | 20 |
| front-extra (4/4) | 4 | 14 | 4 | 6 | 10 | 13 | 12 |
| fisheye | 8 | 13 | 15 | 12 | 5 | 5 | 6 |
| fisheye-extra (4/4) | 4 | 0 | 3 | 4 | 3 | 0 | 0 |
| **Tổng** | **28** | **65** | **36** | **30** | **48** | **48** | **38** |

Tổng **265 shape và 112 tag**, gồm 20 ảnh bắt buộc và 8 ảnh extra. B11 có bốn tag và không có shape; không thêm lane cho vạch ô đỗ.

Đã có drivable ở B04, B06, B08, B09, B12. B04 và B09 có direct; B06, B08, B12 hiện chỉ có alternative nên cần kiểm ý nghĩa vùng và biên polygon khi self-QC.

## 3. Bảng ca khó

| Ảnh | Nhãn | Phân vân giữa | Đã chọn / hiện trạng | Lý do |
| --- | --- | --- | --- | --- |
| B05, B06 | lane.position | L1/R1 / other | other trong bộ bàn giao | Camera hai bên phải dùng other |
| B10 | camera | phải / trái | fisheye_left | Hậu tố MVL xác định camera trái |
| B12 | Tag ảnh | thiếu / bổ sung | fisheye_right, day, overcast, real | Hậu tố MVR và đối chiếu ảnh gốc |
| B11 | lane | vạch ô đỗ / vạch làn | Không có lane | Guideline không gán vạch ô đỗ thành lane |
| A05 | lane | vạch sơn / mép đường | Export mới có R1, chưa có L1 | Ngoại lệ không thấy vạch đã được coach xác nhận; cần rà R1 trong export mới trước khi coi là vạch sơn |
| A14 | lane.position | hai L1 / vạch riêng biệt | Bản mới có R1; còn position L1 trùng | Đã hết cảnh báo thiếu R1, cần kiểm hai đường L1 |
| A01, A03, A04, A06, A08, A09, A11, A14 | lane | đường tách / position trùng | Chờ kiểm tay | Lint gợi ý, không chứng minh mọi đường trùng đều sai |
| B02, B04, B08 | Polyline | đoạn ngắn / cung cong chưa đủ điểm | Chờ kiểm tay | Không chèn điểm trên đường thẳng chỉ để xóa cảnh báo |

## 4. Phiếu QC 1 nhận được và phản hồi

Người QC: **Nguyễn Minh Nam Hải — 2A202603076**. Đã có phiếu hỗ trợ kiểm revision 4; phiếu đó chưa có xác nhận hoặc quyết định cuối của Hải và chưa cập nhật hình học revision 5.

### Bản export mới — revision 5

Đã gộp front_12 (1).zip (A01–A12), front_8.zip (A13–A16), fisheye_4.zip (B01–B08) và fisheye_extra.zip (B09–B12). Giữ bản trước trong [r4/](r4/). Bộ mới giữ các hình học của export mới, chỉ chuẩn hóa các thuộc tính và chiều theo [local-fixes.json](local-fixes.json).

| Mã | Số cảnh báo | Nội dung hiện tại |
| --- | ---: | --- |
| QC1-02, nặng | 2 | A05 thiếu L1; A09 thiếu R1 |
| QC1-05, chính | 6 | A01 L2; A04 L1; A06 L1; A09 L2; A11 L1; A14 L1 trùng |
| QC1-11, chính | 5 | B02 còn ba road_curb hai điểm; B04 và B08 mỗi ảnh một lane ba điểm |
| **Tổng** | **13** | **2 nặng theo lint, 11 chính, 0 nhẹ** |

Coach đã xác nhận ngoại lệ không thấy vạch ở A05 theo thông tin học viên. Tuy nhiên export mới lại có một lane R1 tại A05; bộ revision 5 giữ theo export mới, cần rà để tránh gán mép đường thành lane. Ngoại lệ A05 không áp dụng cho cảnh báo A09 thiếu R1; chưa có xác nhận ngoại lệ cho A09.

Các cảnh báo chính là gợi ý cần kiểm ảnh, chưa phải lỗi đã được GT xác nhận. Chưa tính tỉ lệ lỗi chính hoặc gán quyết định Release. [Nhật ký lint](lint-results.txt) là kết quả hiện tại; các thống kê revision 4 trong hồ sơ QC cũ là lịch sử.

## 5. Kiểm file export

| File | Mở lại được? | Số shape ảnh 1 (job / file) | Số shape ảnh 2 (job / file) | Tag camera trong XML? |
| --- | --- | --- | --- | --- |
| [kieuquangtruong-front.zip](kieuquangtruong-front.zip) | Có, 16 ảnh | A01: chưa đối chiếu giao diện / 12 | A02: chưa đối chiếu giao diện / 8 | Có, 16/16 |
| [kieuquangtruong-fisheye.zip](kieuquangtruong-fisheye.zip) | Có, 12 ảnh | B01: chưa đối chiếu giao diện / 3 | B02: chưa đối chiếu giao diện / 13 | Có, 12/12 |

Định dạng CVAT for images 1.1; mỗi ZIP có annotations.xml, không kèm ảnh. Tên ảnh giữ nguyên; ID đánh lại liên tục trong từng ZIP. [Manifest](kieuquangtruong-manifest.json) revision 5 ghi số ảnh, số nhãn, SHA-256 và kết quả lint. [Nhật ký lint](lint-results.txt) lưu từng cảnh báo. Đã kiểm SHA-256 của hai ZIP khớp manifest.

Nguồn lần này:

| Nguồn | Phạm vi |
| --- | --- |
| front_12 (1).zip, export 12:27:32 UTC+7 | A01–A12 |
| front_8.zip, export 11:22:59 UTC+7 | A13–A16 |
| fisheye_4.zip (12:34:50 UTC+7) và fisheye_extra.zip (10:56:42 UTC+7) | B01–B12 |

Tên `front_8.zip` không phản ánh số lượng hoặc nhóm ảnh hiện tại; đã đọc tên ảnh bên trong để chọn nguồn. Chi tiết nguồn và hash tại [export-provenance.json](export-provenance.json). Chưa xác minh trực tiếp trạng thái completed của các job trên giao diện.

## 6. Ảnh chụp màn hình

Chưa nhận được ảnh chụp CVAT cuối để đính kèm. Bốn minh chứng theo mẫu cần bổ sung:

1. Tab Labels của project.
2. Một ảnh front đã gán, thể hiện các nhóm label yêu cầu.
3. Một ảnh fisheye phóng to polyline cong.
4. Đoạn XML có `<polyline label="lane">` cùng thuộc tính.

Không dùng ảnh chồng nhãn của bản nháp cũ để đại diện bản cuối. Hai ZIP nhãn không kèm ảnh dữ liệu.

## 7. Tự đánh giá

Các vấn đề cần chú ý qua quá trình tự kiểm là thiếu hoặc nhầm camera/tag (QC1-01), sai position ở camera bên (QC1-03), thiếu L1/R1 hoặc position trùng (QC1-02/QC1-05), và polyline fisheye chưa thể hiện đủ cung cong (QC1-11). Các cảnh báo L1/R1 đã giảm sau cập nhật front. Chưa có QC tay để khẳng định loại lỗi mắc nhiều nhất trong toàn bộ buổi.

Để tránh lặp lại ở Ngày 20, đặt camera trước khi vẽ, kiểm hậu tố FV/RV/MVL/MVR và sửa giá trị mặc định ngay sau khi tạo hình. Kiểm tâm vạch, chiều gần → xa, điểm dừng, nối đoạn che và occluded; camera bên luôn dùng other. Với fisheye, đặt thêm điểm tại vị trí cong thực tế, không lấy vạch ô đỗ hoặc vạch qua đường làm lane.

Nhãn lane là mục tiêu hình học/thuộc tính cho nhận biết làn; polygon drivable và crosswalk cung cấp mục tiêu phân vùng; box đèn/biển phục vụ phát hiện đối tượng; tag giúp phân nhóm và đánh giá điều kiện camera, môi trường. Ví dụ đã sửa: lane B05 gán L1 trong ảnh camera trái sẽ khiến mục tiêu huấn luyện mang ý nghĩa biên trái của làn đang đi sai quy ước. Sửa thành other giúp mục tiêu đúng nghĩa. Camera B10 ghi nhầm hướng cũng làm sai thống kê và đánh giá theo camera.

## 8. Row-anchor (tùy chọn)

Không thực hiện trong báo cáo này. Đây là hoạt động tùy chọn, không thay QC chéo hoặc kiểm hình học.

## Trạng thái trước nộp

Đã có hai ZIP gộp đủ ảnh bắt buộc và extra, manifest và báo cáo có số liệu thật. Mẫu [Phiếu QC chéo](kieuquangtruong-qc1-for-chuaxacnhan.md) còn chờ tên bạn và kết quả kiểm thực tế. Cần bổ sung ảnh minh chứng, đối chiếu job completed, rà các cảnh báo chính; ngoại lệ không thấy vạch của A05 đã được coach xác nhận. Báo cáo không xác nhận đạt hoặc Release khi các mục này chưa được kiểm chứng.


### Thông tin xác nhận bổ sung

Học viên thông báo coach đã xác nhận ngoại lệ A05 không quan sát được vạch sơn phân làn. Người QC: **Nguyễn Minh Nam Hải — 2A202603076**. Chưa có quyết định Release/Hold/Rework hoặc nội dung phiếu từ người QC, nên báo cáo chưa gán quyết định thay người kiểm.


### Phiếu QC được hỗ trợ lập tại workspace

Đã cập nhật [phiếu Nam Hải kiểm bài Trường](nguyenminhnamhai-qc1-for-kieuquangtruong.md) theo output revision 5: 28 ảnh, 265 shape, 112 tag, 13 cảnh báo máy. Ghi riêng ngoại lệ A05, ba road_curb B02 đã kiểm thực tế và giải thích các cặp vạch đôi. A04 có hai L1 mang kind single/double; A09 thiếu R1; lane B04/B08 còn cần xác nhận. Phiếu chờ Nam Hải xác nhận kết luận cuối, chưa tự ghi Release.

### Xác nhận kiểm trực quan road_curb B02

Theo xác nhận của học viên sau khi kiểm thực tế, ba đoạn road_curb B02 bắt đầu gần (1001,515), (1190,659), (1056,552) là các đoạn ngắn, hai điểm đã bám đúng mép đường nên giữ nguyên. Ba cảnh báo QC1-11 ít điểm được ghi nhận là đã rà trực quan, không tính thành lỗi hình học chỉ vì có hai điểm. Đây là xác nhận do học viên cung cấp; không suy rộng sang lane B04/B08 hoặc các cảnh báo position. Giữ nguyên ZIP, manifest và log lint để truy vết.

