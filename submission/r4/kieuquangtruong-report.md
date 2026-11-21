# Báo cáo cá nhân — Ngày 11 · Road Elements & SVM

Họ tên: **Kiều Quang Trường**. Mã học viên: **03205** (theo tên thư mục bài làm).  
Guideline: **Road v1.0**. Lần đóng gói: **revision 4**, ngày **21/11/2026**.  
Phiên bản CVAT: chưa có ảnh Help → About để xác nhận.  
Thời điểm export mới nhất: front A01–A12 **11:21:48**, front-extra A13–A16 **11:22:59**, ngày 21/11/2026 (UTC+7). Fisheye giữ nguyên bộ revision 3 đã kiểm vì không tìm thấy export fisheye mới hơn trong các ZIP hiện tại.

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
| front | 12 | 40 | 14 | 8 | 30 | 30 | 20 |
| front-extra (4/4) | 4 | 14 | 4 | 6 | 10 | 13 | 12 |
| fisheye | 8 | 13 | 16 | 10 | 5 | 5 | 6 |
| fisheye-extra (4/4) | 4 | 0 | 3 | 4 | 3 | 0 | 0 |
| **Tổng** | **28** | **67** | **37** | **28** | **48** | **48** | **38** |

Tổng **266 shape và 112 tag**, gồm 20 ảnh bắt buộc và 8 ảnh extra. B11 có bốn tag và không có shape; không thêm lane cho vạch ô đỗ.

Đã có drivable ở B04, B06, B08, B09, B12. B04 và B09 có direct; B06, B08, B12 hiện chỉ có alternative nên cần kiểm ý nghĩa vùng và biên polygon khi self-QC.

## 3. Bảng ca khó

| Ảnh | Nhãn | Phân vân giữa | Đã chọn / hiện trạng | Lý do |
| --- | --- | --- | --- | --- |
| B05, B06 | lane.position | L1/R1 / other | other trong bộ bàn giao | Camera hai bên phải dùng other |
| B10 | camera | phải / trái | fisheye_left | Hậu tố MVL xác định camera trái |
| B12 | Tag ảnh | thiếu / bổ sung | fisheye_right, day, overcast, real | Hậu tố MVR và đối chiếu ảnh gốc |
| B11 | lane | vạch ô đỗ / vạch làn | Không có lane | Guideline không gán vạch ô đỗ thành lane |
| A05 | lane | vạch sơn / mép đường | Không gán L1/R1 sau đối chiếu ảnh gốc | Không quan sát được vạch sơn phân làn; đường R1 mới vẽ đi gần mép đường, không có vệt sơn trắng rõ tương ứng nên đã bỏ. Coach đã xác nhận ngoại lệ QC1-02 theo thông tin học viên cung cấp |
| A14 | lane.position | hai L1 / vạch riêng biệt | Bản mới có R1; còn position L1 trùng | Đã hết cảnh báo thiếu R1, cần kiểm hai đường L1 |
| A01, A03, A04, A06, A08, A09, A11, A14 | lane | đường tách / position trùng | Chờ kiểm tay | Lint gợi ý, không chứng minh mọi đường trùng đều sai |
| B02, B04, B08 | Polyline | đoạn ngắn / cung cong chưa đủ điểm | Chờ kiểm tay | Không chèn điểm trên đường thẳng chỉ để xóa cảnh báo |

## 4. Phiếu QC 1 nhận được và phản hồi

Người kiểm bài của tôi: **Nguyễn Minh Nam Hải — 2A202603076** (học viên cung cấp). Quyết định QC, danh sách lỗi từ người kiểm và người tôi được ghép kiểm: **chưa được cung cấp**. Không dùng lint tự kiểm thay cho Phiếu QC 1 của bạn cùng lớp.

Các lần cập nhật:

- Revision 1: gộp đủ 28 ảnh và sửa các thuộc tính có thể xác định từ guideline và tên ảnh.
- Revision 2: bổ sung 8 drivable và 1 lane trong export fisheye; lưu bản trước tại [r1/](r1/).
- Revision 3: dùng front A01–A12 mới trong `front_12 (1).zip` và A13–A16 mới trong `front_8.zip`. Bộ front đã có L1/R1 ở A06, A07, A09, A16; còn A05 và A14 cần xác minh. Lưu revision 2 tại [r2/](r2/).

Sửa cục bộ đã áp dụng trong bộ gộp: camera B10; bốn tag B12; position=other cho lane camera bên; edge_zone theo tâm hộp và bán kính checker; đảo thứ tự điểm lane front có chiều xa → gần. Giữ nguyên vị trí điểm khi đảo chiều. Chi tiết lần chuẩn hóa hiện tại có trong [local-fixes.json](local-fixes.json); lịch sử trước đó nằm trong r1/r2.

Kết quả lint hiện tại:

| Mã | Số cảnh báo | Nội dung |
| --- | ---: | --- |
| QC1-02, nặng | 2 | Chỉ A05 thiếu L1/R1 trên cấu trúc XML; ảnh gốc không thấy vạch sơn, coach đã xác nhận ngoại lệ theo thông tin học viên cung cấp |
| QC1-05, chính | 9 | A01, A03, A04 (2), A06, A08, A09, A11, A14: position trùng |
| QC1-11, chính | 8 | B02: sáu road_curb ít điểm; B04 và B08: mỗi ảnh một lane ít điểm |
| **Tổng** | **19** | **2 nặng theo lint ở A05, 17 chính, 0 nhẹ** |

Lần kiểm cuối còn 19 cảnh báo; hai cảnh báo QC1-02 ở A05 là ca không thấy vạch đã được coach xác nhận ngoại lệ, không kết luận học viên bỏ sót. Đây là cảnh báo tự kiểm, chưa phải số lỗi được xác nhận so với GT. Không tính tỉ lệ lỗi chính hoặc kết luận Release từ các số này. Ngoại lệ QC1-02 tại A05 đã được coach xác nhận theo thông tin học viên cung cấp.


### Kết quả kiểm cuối — revision 4

Đã đọc hai export front mới nhất, đối chiếu trực tiếp ảnh gốc A05 và chạy lại lint trên hai ZIP cuối. A14 đã có R1. A05 không quan sát được vạch sơn phân làn: không vẽ L1/R1 theo mép đường hoặc xe đỗ; đã bỏ một lane R1 không có vệt sơn rõ tương ứng. Bản trước khi bỏ lane được giữ tại `../output/front-before-A05-review-r4.zip`. Chỉ riêng A05 còn hai cảnh báo QC1-02 của checker; coach đã xác nhận ngoại lệ theo thông tin học viên cung cấp. Giữ nguyên hai cảnh báo máy trong manifest để truy vết; không tính chúng là lỗi bỏ sót sau xác nhận.

Bộ revision 3 được lưu ở [r3/](r3/). Những cảnh báo chính còn lại vẫn là gợi ý kiểm tay, không coi là lỗi đã được GT xác nhận.

## 5. Kiểm file export

| File | Mở lại được? | Số shape ảnh 1 (job / file) | Số shape ảnh 2 (job / file) | Tag camera trong XML? |
| --- | --- | --- | --- | --- |
| [kieuquangtruong-front.zip](kieuquangtruong-front.zip) | Có, 16 ảnh | A01: chưa đối chiếu giao diện / 12 | A02: chưa đối chiếu giao diện / 8 | Có, 16/16 |
| [kieuquangtruong-fisheye.zip](kieuquangtruong-fisheye.zip) | Có, 12 ảnh | B01: chưa đối chiếu giao diện / 3 | B02: chưa đối chiếu giao diện / 13 | Có, 12/12 |

Định dạng CVAT for images 1.1; mỗi ZIP có annotations.xml, không kèm ảnh. Tên ảnh giữ nguyên; ID đánh lại liên tục trong từng ZIP. [Manifest](kieuquangtruong-manifest.json) revision 4 ghi số ảnh, số nhãn, SHA-256 và kết quả lint. [Nhật ký lint](lint-results.txt) lưu từng cảnh báo. Đã kiểm SHA-256 của hai ZIP khớp manifest.

Nguồn lần này:

| Nguồn | Phạm vi |
| --- | --- |
| front_12 (1).zip, export 11:21:48 UTC+7 | A01–A12 |
| front_8.zip, export 11:22:59 UTC+7 | A13–A16 |
| Bộ fisheye revision 3 đã kiểm, giữ nguyên nhãn | B01–B12 |

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

Đã có [phiếu QC hỗ trợ cho Nguyễn Minh Nam Hải](nguyenminhnamhai-qc1-for-kieuquangtruong.md), ghi kết quả kiểm toàn bộ cấu trúc 28 ảnh, 266 shape và 112 tag; đủ 12 mã QC với phạm vi đã kiểm và các mục cần kiểm hình học. Người QC được chỉ định: Nguyễn Minh Nam Hải — 2A202603076. Phiếu do Codex hỗ trợ lập, chưa có xác nhận hoặc quyết định cuối của Hải. Phiếu này là chiều Hải kiểm bài Trường, không thay phiếu Trường kiểm bài bạn khác.

