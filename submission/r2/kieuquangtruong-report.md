# Báo cáo cá nhân — Ngày 11 · Road Elements & SVM

Họ tên: **Kiều Quang Trường** (theo tên thư mục bài làm).  
Mã học viên: **03205** (theo tên thư mục; cần đối chiếu mã chính thức).  
Phiên bản CVAT (Help → About): **chưa có thông tin xác nhận**.  
Guideline: **Road v1.0**. Lần đóng gói: **revision 2**.  
Ngày lập báo cáo: **21/11/2026**, múi giờ Asia/Bangkok (UTC+7).  
Thời điểm export gốc: **10:26:41–10:56:42 ngày 21/11/2026 (UTC+7)**, lấy từ `meta/dumped` trong XML.  
Số lần nộp lại sau Rework: **chưa có thông tin QC chéo để xác nhận**.

**Trạng thái bàn giao:** đã xử lý và đóng gói đủ 28 ảnh, gồm 20 ảnh bắt buộc và 8 ảnh extra. Bộ ZIP mở lại được, không kèm ảnh, đủ bốn tag cho từng ảnh. Chưa kết luận Release: còn 24 cảnh báo lint cần rà soát, thiếu kết quả QC chéo và ảnh chụp giao diện CVAT. Các sửa đổi cục bộ được ghi ở mục 4; chưa xác minh đã đồng bộ lại trên CVAT.

## Kiểm tra lần sửa thứ hai

Hai bản export fisheye đã được cập nhật lúc 10:56 ngày 21/11/2026; hai bản front giữ nguyên mã SHA-256 so với revision 1. Đã bổ sung 8 drivable và 1 lane, tăng từ 245 lên 254 shape. Lint sau chuẩn hóa thuộc tính vẫn còn 24 cảnh báo (8 nặng, 16 chính). Bộ hiện tại là revision 2, chưa phải kết luận Release của người kiểm.

Các vấn đề thuộc tính vẫn xuất hiện trong export mới trước chuẩn hóa: B05 còn lane.position=L1; lane mới B06 có position=R1; B10 còn camera=fisheye_right; B12 còn thiếu bốn tag. Bộ ZIP bàn giao đã sửa lại các giá trị này, nhưng cần đồng bộ trên CVAT.

## 1. Kiểm project

| Mục | Đối chiếu Road v1.0 | Ghi chú |
| --- | --- | --- |
| 6 label, đúng kiểu | Metadata export khai báo đúng sáu label hình học | `lane`, `road_curb`: polyline; `crosswalk`, `drivable`: polygon; `traffic_light`, `traffic_sign`: rectangle |
| Thuộc tính và giá trị | Có khai báo thuộc tính theo cấu hình Road v1.0; lint không báo label hoặc giá trị lạ | Đã sửa `position` của lane B05 từ L1 thành other; cần kiểm hình học và ý nghĩa thuộc tính trên từng ảnh |
| 4 tag ảnh | Đủ camera, timeofday, weather, source trên 28/28 ảnh sau sửa cục bộ | Đã sửa camera B10; bổ sung bốn tag B12 |

Tag ảnh được lưu thành `<tag label="camera"><attribute name="value">…</attribute></tag>` và tương tự cho ba tag còn lại. Mỗi ảnh có một giá trị cho mỗi tag.

Cách kết thúc polyline trên phiên bản CVAT đang dùng: **cần bổ sung thao tác thực tế đã dùng; chưa xác minh từ giao diện**. Sau khi vẽ cần kiểm chiều gần → xa, tâm vệt sơn, điểm dừng và thuộc tính.

Cách đặt tag trên phiên bản CVAT đang dùng: chọn label kiểu Tag, đặt thuộc tính `value`, rồi lưu; **cần đối chiếu thao tác thực tế trên phiên bản đang dùng**.

## 2. Số lượng đã gán

Số liệu được đếm trực tiếp từ bốn bản export CVAT cuối do học viên cung cấp, không dùng bản nháp nhãn đã tạo trước đó. Số shape không bao gồm tag.

| Task | Ảnh | lane | road_curb | crosswalk | drivable | traffic_light | traffic_sign |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| front | 12 | 38 | 15 | 8 | 30 | 24 | 16 |
| front-extra (4/4) | 4 | 11 | 6 | 6 | 10 | 13 | 12 |
| fisheye | 8 | 13 | 16 | 10 | 5 | 5 | 6 |
| fisheye-extra (4/4) | 4 | 0 | 3 | 4 | 3 | 0 | 0 |
| **Tổng** | **28** | **62** | **40** | **28** | **48** | **42** | **34** |

Tổng **254 shape** và **112 tag** trong hai ZIP bàn giao. B11 có bốn tag và không có shape; đây là ca vạch ô đỗ, không tự thêm lane chỉ để tăng số lượng.

**Kiểm bản sửa:** đã có 8 polygon drivable: B04 (1 direct), B06 (2 alternative), B08 (2 alternative), B09 (1 direct), B12 (2 alternative). Nhóm drivable đã hiện diện ở B04, B06, B08 và B12 như trọng tâm trong manifest. Cần kiểm tiếp cách chia direct/alternative ở B06, B08, B12 vì các polygon tại ba ảnh này đều đang mang alternative; lint không xác minh ý nghĩa hoặc độ bám biên của polygon.

## 3. Bảng ca khó

Bảng ghi nhận ca khó và vấn đề phát hiện khi kiểm bản export; không thay cho xác nhận của coach.

| Ảnh | Nhãn | Phân vân giữa | Đã chọn / hiện trạng | Lý do và việc cần làm |
| --- | --- | --- | --- | --- |
| B05 | lane.position | L1 / other | Sửa thành other trong ZIP bàn giao | Camera MVL là fisheye_left; mục 4 guideline quy định camera hai bên luôn dùng other |
| B10 | camera | fisheye_right / fisheye_left | Sửa thành fisheye_left | Hậu tố MVL xác định camera trái, không phụ thuộc hướng người xem ảnh |
| B12 | Tag ảnh | Thiếu tag / bổ sung theo ảnh | fisheye_right, day, overcast, real | Hậu tố MVR; ảnh gốc ban ngày, ánh sáng âm u, mặt đường không thể hiện mưa rõ; nguồn WoodScape thật |
| B11 | lane | Vạch ô đỗ / vạch phân làn | Không có lane trong export | Không gán vạch ô đỗ thành lane theo quy tắc fisheye |
| B02, B04, B05 | edge_zone | false / true | Bật cho các box có tâm ngoài 80% bán kính theo quy ước checker | Dùng tâm ảnh và bán kính bằng nửa chiều rộng; cần người kiểm đối chiếu vòng ảnh thực tế |
| A05, A06, A07, A09, A14, A16 | lane.position | Không thấy vạch / thiếu hoặc sai position | Chưa tự thêm hoặc đổi nhãn | Cần mở ảnh kiểm L1/R1; nếu thực sự không thấy vạch thì xin coach xác nhận ngoại lệ QC1-02 |
| A01, A03, A04, A08, A09, A11, A14 | lane | Vạch bị tách / nhiều đường trùng position | Giữ hình học gốc, chờ rà soát | Lint phát hiện position lặp; phải nhìn ảnh trước khi gộp hoặc đổi position |
| B02, B04, B08 | Polyline | Đoạn ngắn thẳng / cung cong chưa đủ điểm | Chưa tự chèn điểm nội suy | Thêm điểm nội suy trên đường thẳng không chứng minh bám cung thật hoặc lệch ≤ 5 px; cần sửa theo pixel ảnh |

## 4. Phiếu QC 1 nhận được

Bạn kiểm lô của tôi: **chưa được cung cấp**.  
Quyết định nhận được: **chưa có phiếu QC chéo**.  
Người tôi kiểm lô: **chưa được cung cấp**.

Không dùng kết quả `lint` để thay Phiếu QC 1 của bạn cùng lớp, không coi số cảnh báo là số lỗi đã được người kiểm xác nhận.

### Những sửa đổi cục bộ đã thực hiện khi chuẩn bị bộ nộp

| Mã | Nội dung | Đã sửa trong ZIP? | Ghi chú |
| --- | --- | --- | --- |
| QC1-01 | B10: camera từ fisheye_right → fisheye_left | Có | Theo MVL |
| QC1-01 | B12: bổ sung camera, timeofday, weather, source | Có | Bốn tag, mỗi tag có thuộc tính value |
| QC1-03 | B05: L1 → other; lane mới B06: R1 → other | Có | Quy tắc camera hai bên |
| QC1-04 | Đảo thứ tự điểm của hai lane A08 và một lane A16 | Có | Giữ nguyên vị trí điểm, đổi chiều gần → xa |
| QC1-11 | Bật edge_zone cho một đèn B02, một biển B04 và ba biển B05 | Có | Theo quy ước bán kính của checker |

Bản gốc bốn ZIP vẫn được giữ nguyên ở thư mục gốc. Bộ bàn giao revision 1 được lưu tại [r1/](r1/). Hai ZIP gộp trước lần sửa đầu được lưu tại `../output/submission-before-fixes/`. Chi tiết sửa thuộc tính có trong [local-fixes.json](local-fixes.json). Những thay đổi này cần được áp dụng lại trên CVAT trước lần export chính thức kế tiếp.

### Kết quả kiểm tự động còn tồn tại

| Nhóm cảnh báo | Số lượng | Ảnh |
| --- | ---: | --- |
| QC1-02 mức nặng: thiếu L1/R1 | 8 | A05: L1, R1; A06: L1, R1; A07: L1; A09: R1; A14: R1; A16: R1 |
| QC1-05 mức chính: position trùng | 8 | A01, A03, A04 (hai vị trí), A08, A09, A11, A14 |
| QC1-11 mức chính: polyline fisheye ít điểm | 8 | B02: sáu road_curb; B04: một lane; B08: một lane |
| **Tổng** | **24** | **8 nặng, 16 chính, 0 nhẹ** |

Tệp [lint-results.txt](lint-results.txt) ghi đầy đủ từng cảnh báo. Chưa có GT và chưa QC tay nên không tính tỉ lệ lỗi chính, không kết luận điểm hoặc Release. Cảnh báo QC1-02 chỉ được miễn khi coach xác nhận không thấy vạch trên ảnh.

## 5. Kiểm file export

Hai ZIP có định dạng CVAT for images 1.1, mỗi ZIP chứa `annotations.xml`, tên ảnh giữ nguyên; ID được đánh lại liên tục trong từng ZIP. Đã mở lại bằng `road_check lint` và tạo [manifest bàn giao](kieuquangtruong-manifest.json).

| File | Mở lại được? | Số shape ảnh 1 (job / file) | Số shape ảnh 2 (job / file) | Tag camera trong XML? |
| --- | --- | --- | --- | --- |
| [kieuquangtruong-front.zip](kieuquangtruong-front.zip) | Có, 16 ảnh | A01: chưa đối chiếu giao diện / 12 | A02: chưa đối chiếu giao diện / 8 | Có, đủ 16/16 ảnh |
| [kieuquangtruong-fisheye.zip](kieuquangtruong-fisheye.zip) | Có, 12 ảnh | B01: chưa đối chiếu giao diện / 3 | B02: chưa đối chiếu giao diện / 13 | Có, đủ 12/12 ảnh |

`contains_images = false` ở cả hai ZIP. Mọi ảnh có đủ bốn tag sau sửa cục bộ. Chưa kiểm trực tiếp trạng thái completed của job trên CVAT; học viên cho biết đã hoàn tất các phần, nhưng XML export không xác nhận trạng thái workflow này.

Nguồn gộp được xác định theo tên ảnh thay vì tên ZIP:

| ZIP gốc | Phạm vi thực tế | Giờ export UTC+7 |
| --- | --- | --- |
| front_12.zip | A01–A12 | 21/11/2026 10:26:41 |
| fisheye_4.zip | A13–A16 | 21/11/2026 10:27:12 |
| front_8.zip | B01–B08 | 21/11/2026 10:56:25 |
| fisheye_extra.zip | B09–B12 | 21/11/2026 10:56:42 |

`front_12 (1).zip` là bản export trùng danh sách ảnh và thời điểm với `front_12.zip`; không nhập thêm để tránh nhân đôi dữ liệu. Thông tin truy vết có trong [export-provenance.json](export-provenance.json).

## 6. Ảnh chụp màn hình

**Chưa có ảnh chụp giao diện CVAT cuối để đính kèm.** Cần bổ sung bốn minh chứng sau trước khi nộp báo cáo chính thức:

1. Tab Labels của project, thể hiện sáu label hình học và bốn tag.
2. Một ảnh front đã gán đủ các nhóm yêu cầu, hiển thị làn, vùng lái và đèn/biển khi có.
3. Một ảnh fisheye, phóng to đoạn polyline cong để thấy độ bám vạch và điểm điều khiển.
4. Một đoạn XML export có `<polyline label="lane">` cùng thuộc tính.

Không dùng ảnh chồng nhãn của bản nháp trước để đại diện cho bản export cuối. Khi bổ sung ảnh minh chứng, cần làm theo hướng dẫn kênh nộp của coach; hai ZIP nhãn vẫn không kèm ảnh dữ liệu.

## 7. Tự đánh giá

Trong lần kiểm export này, nhóm vấn đề cần ưu tiên là QC1-01 (camera/tag), QC1-03 (position) và QC1-02 (L1/R1). Sai camera B10 và position B05 đã được sửa trong bộ ZIP; các cảnh báo thiếu L1/R1 còn cần kiểm ảnh hoặc xác nhận của coach. Chưa có thống kê kiểm tay để khẳng định nhóm nào là lỗi mắc nhiều nhất trong toàn bộ buổi học.

Để tránh lặp lại ở Ngày 20, cần đặt camera trước khi vẽ; kiểm hậu tố FV/RV/MVL/MVR; kiểm thuộc tính mặc định ngay sau mỗi hình; camera hai bên luôn dùng position=other. Với fisheye, tăng điểm tại nơi cung cong thật, kiểm cả đoạn gần thân xe và vòng ngoài. Với vạch đứt hoặc vạch bị che, kiểm xem có bị tách thành nhiều đường hay chưa bật occluded khi nối qua đoạn che.

Nhãn lane cung cấp vị trí hình học và thuộc tính để huấn luyện mô hình nhận biết làn; polygon drivable/crosswalk cung cấp vùng mục tiêu cho bài toán phân vùng, còn box đèn/biển cung cấp vị trí và lớp cho phát hiện đối tượng. Tag camera và điều kiện môi trường hỗ trợ chia nhóm dữ liệu và đánh giá theo từng điều kiện.

Ví dụ lỗi đã sửa: B05 là camera trái nhưng một lane được gán L1. Nếu dùng nhãn này để tạo mục tiêu theo position, mô hình có thể học vạch trong ảnh bên hông như biên trái của làn xe đang đi, sai quy ước Road v1.0. Chuyển sang other giúp mục tiêu huấn luyện phù hợp với ý nghĩa camera. Tương tự, camera B10 bị ghi nhầm bên sẽ làm sai thống kê và đánh giá theo hướng camera.

## 8. Hoạt động row-anchor (tùy chọn)

Chưa thực hiện trong báo cáo này. Hoạt động tùy chọn, không được dùng để thay thế QC chéo hoặc kiểm hình học.

## Những việc cần hoàn tất trước khi nộp chính thức

- [ ] Rà 8 cảnh báo thiếu L1/R1; sửa nhãn hoặc có xác nhận ngoại lệ của coach.
- [ ] Rà 8 cảnh báo position trùng và 8 cảnh báo polyline fisheye ít điểm.
- [x] Có drivable ở B04, B06, B08, B12; đã đếm lại từ export mới.
- [ ] Kiểm biên polygon và direct/alternative, đặc biệt B06, B08, B12.
- [ ] Đồng bộ các sửa đổi cục bộ trên CVAT, kiểm mọi job completed, đối chiếu hai ảnh mỗi ZIP.
- [ ] Điền phiên bản CVAT, xác nhận mã học viên và bổ sung bốn ảnh minh chứng.
- [ ] Nhận Phiếu QC chéo, xử lý các lỗi; hoàn thành Phiếu QC 1 cho bạn được ghép.
- [ ] Sau khi sửa hoặc export lại, tạo lại manifest đúng hai ZIP cuối; tăng revision nếu đây là lần nộp lại sau Rework.

