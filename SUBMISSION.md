# Nộp bài (hạn 16:35, Ngày 11)

Nộp đúng 5 file lên kênh nộp bài coach chỉ định. Không gửi file GT, không gửi ảnh và không đăng file lên kênh chung của lớp.

- [x] [kieuquangtruong-front.zip](submission/kieuquangtruong-front.zip): CVAT for images 1.1, **không kèm ảnh**; đủ A01–A16, gồm 12 ảnh bắt buộc và 4 ảnh extra.
- [x] [kieuquangtruong-fisheye.zip](submission/kieuquangtruong-fisheye.zip): CVAT for images 1.1, **không kèm ảnh**; đủ B01–B12, gồm 8 ảnh bắt buộc và 4 ảnh extra.
- [x] [kieuquangtruong-manifest.json](submission/kieuquangtruong-manifest.json): revision 5; SHA-256 khớp hai ZIP, `contains_images = false`.
- [ ] Phiếu **Kiều Quang Trường kiểm bài bạn được ghép**: [mẫu chờ điền](submission/kieuquangtruong-qc1-for-chuaxacnhan.md). Chưa có tên và kết quả kiểm bài bạn. [Phiếu hỗ trợ Hải kiểm bài Trường](submission/nguyenminhnamhai-qc1-for-kieuquangtruong.md) là chiều ngược lại, chưa thay file này.
- [x] [kieuquangtruong-report.md](submission/kieuquangtruong-report.md): đã điền số liệu 28 ảnh, 265 shape, 112 tag; còn thiếu ảnh minh chứng CVAT và kết quả QC chéo chính thức.

Học viên: **Kiều Quang Trường — 03205** (mã theo thư mục bài làm). Người QC bài Trường: **Nguyễn Minh Nam Hải — 2A202603076**. Các dấu tích trên xác nhận file đã chuẩn bị, **chưa xác nhận đã gửi lên kênh nộp**.

Trước khi nộp:

- [ ] Mọi job ở trạng thái **completed**: chưa xác minh trực tiếp trên giao diện CVAT.
- [x] Đã mở lại hai ZIP và chạy `road_check lint`: còn 2 cảnh báo QC1-02: A05 thiếu L1 và A09 thiếu R1; **coach đã xác nhận ngoại lệ A05 theo thông tin học viên; A09 chưa có xác nhận ngoại lệ**, đã ghi vào báo cáo và phiếu hỗ trợ QC. Còn 11 cảnh báo chính cần kiểm tay; chưa phải kết luận Release. Xem [nhật ký lint](submission/lint-results.txt).
- [x] Đã tạo manifest bằng `lint --manifest submission/kieuquangtruong-manifest.json --name kieuquangtruong --revision 5`; cả hai file có `contains_images = false`.
- [x] Đủ bốn tag trên 28/28 ảnh; camera đã đối chiếu hậu tố tên ảnh trong bộ ZIP cuối.
- [ ] Báo cáo đã có bảng kiểm file tại mục 5; **chưa có bốn ảnh chụp minh chứng CVAT** tại mục 6.

Sửa sau buffer: trước khi export lại, chép hai zip và manifest cũ vào thư mục `r1/` trên máy (manifest cũ đổi tên thành `<tên>-manifest-r1.json`). Sau đó nộp hai file zip **cùng tên** và manifest mới với `--revision 2`. Ghi vào mục 4 của báo cáo đã sửa những gì.


### Cập nhật kiểm thực tế

- [x] Ba đoạn road_curb B02 tại (1001,515), (1190,659), (1056,552) đã được học viên xác nhận kiểm trực quan: đoạn ngắn, hai điểm bám đúng mép đường, giữ nguyên. Cảnh báo máy vẫn giữ trong log; không coi ít điểm tự động là lỗi. Không áp dụng xác nhận này cho lane B04/B08 hoặc position trùng.

