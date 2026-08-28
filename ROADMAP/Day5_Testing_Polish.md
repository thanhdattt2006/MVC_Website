# DAY 5: TESTING, BUG FIXING VÀ HOÀN THIỆN (POLISHING)

**Mục tiêu**: Đây là ngày sống còn, đóng băng tính năng mới (Code Freeze). Chạy nước rút rà soát lỗi từ lớn đến nhỏ, chuẩn bị kịch bản demo cho Giám khảo (BGK).

## Phase 5.1: Đóng Băng Tính Năng (Code Freeze)
- `[ ]` Toàn team thống nhất KHÔNG THÊM tính năng mới, dù tính năng đó có vẻ hay.
- `[ ]` Review lại requirement đề thi TechWiz, xem còn sót yêu cầu CHÍNH nào không (VD: Có yêu cầu bắt buộc xuất file PDF không? Bắt buộc gửi Mail không?).
- `[ ]` Nếu thiếu yêu cầu chính yếu -> Cắt cử 1 người giỏi nhất làm cấp tốc, các thành viên khác bắt tay vào bug fix.

## Phase 5.2: QA Testing Toàn Diện
- `[ ]` Dành 1 tiếng đóng vai Người Dùng (End-user) test toàn bộ flow website.
- `[ ]` Cố tình thao tác sai: Nhập chữ vào ô số, để trống trường bắt buộc, upload file cực to, click liên tục vào nút submit.
- `[ ]` Dành 30 phút đóng vai Hacker: Thử sửa URL thay ID của người khác xem có xem/xóa lén được không. (Ví dụ URL là `/order/5` thì gõ thành `/order/6`).
- `[ ]` Nếu có lỗi IDOR (xem lén dữ liệu chéo) này, phải bổ sung ngay logic check quyền sở hữu ở Controller (Day 3).
- `[ ]` Note toàn bộ lỗi tìm thấy vào 1 danh sách.

## Phase 5.3: Xử lý Lỗi (Bug Fixing)
- `[ ]` Ưu tiên 1: Fix các lỗi trắng trang (Error 500) hoặc crash ứng dụng.
- `[ ]` Ưu tiên 2: Fix các lỗi nghiệp vụ (Tính sai tiền, cập nhật nhầm dữ liệu).
- `[ ]` Ưu tiên 3: Fix lỗi hiển thị (Vỡ giao diện, text tràn khung).
- `[ ]` Ưu tiên 4: Cải thiện Micro-interactions (hiệu ứng nhỏ, tooltip).
- `[ ]` Sửa xong lỗi nào, commit và push ngay lập tức: `git commit -m "Fix bug: tính sai tổng tiền đơn hàng"`.

## Phase 5.4: Dọn dẹp Database Production
- `[ ]` Truy cập database Aiven.
- `[ ]` Xoá sạch toàn bộ data rác, text vô nghĩa do việc test lúc đang code tạo ra (Ví dụ tài khoản "asdsd", sản phẩm tên "test 123").
- `[ ]` Chuẩn bị một bộ Database Mẫu (Demo Data) CỰC KỲ SẠCH ĐẸP: Nhập dữ liệu có ý nghĩa, sử dụng ảnh thật, tên người thật, văn phong chỉnh chu. BGK sẽ nhìn vào data này để chấm.
- `[ ]` Phân công 1 người chuyên ngồi nhập Data chuẩn trong lúc 3 người kia fix bug.

## Phase 5.5: Cấu hình Production trên Render
- `[ ]` Vào trang cấu hình Environment của Render.
- `[ ]` Đảm bảo `APP_ENV=production`.
- `[ ]` Đảm bảo `APP_DEBUG=false`. Tuyệt đối không để true khi chấm bài, nếu có lỗi màn hình văng ra đống code sẽ bị trừ điểm nặng. Thiết kế 1 trang Error 500 tùy chỉnh đẹp mắt (`resources/views/errors/500.blade.php`).
- `[ ]` Đảm bảo `APP_URL` đã trỏ đúng domain của Render.

## Phase 5.6: Rà Soát UX/UI
- `[ ]` Đảm bảo không có cái ảnh gốc nào dung lượng 10MB load trên trang chủ kéo sập tốc độ mạng.
- `[ ]` Đảm bảo các link (the `<a>`) đều click được và không trỏ về dấu `#`.
- `[ ]` Đảm bảo font chữ đồng nhất, không lọt phông chữ mặc định của trình duyệt.
- `[ ]` Check màu sắc của text trên nền (Contrast) xem có dễ đọc không.
- `[ ]` Chạy Lighthouse (trên Google Chrome) để quét điểm Performance, Accessibility. Cố gắng đạt màu Xanh.

## Phase 5.7: Chuẩn Bị File Thuyết Trình & Kịch Bản Demo
- `[ ]` Soạn kịch bản Demo: "Mở đầu -> Giới thiệu vấn đề -> Trình diễn Flow 1 (User thường) -> Trình diễn Flow 2 (Admin) -> Tính năng nổi bật -> Kết luận".
- `[ ]` Đặt sẵn các màn hình, trình duyệt ở đúng trang cần demo.
- `[ ]` Phân công 1 người nói giỏi nhất đứng thuyết trình.
- `[ ]` Chuẩn bị Slide giới thiệu (Team, Tech Stack áp dụng, Tại sao chọn mô hình MVC này, Điểm sáng giá trong source code).

## Phase 5.8: Dry Run (Chạy Nháp Demo)
- `[ ]` Toàn team ngồi nghe người đại diện thuyết trình thử.
- `[ ]` Bấm đồng hồ xem có quá giờ cho phép không (nếu có giới hạn thời gian).
- `[ ]` Các thành viên khác đặt câu hỏi đóng vai giám khảo vặn vẹo.
- `[ ]` Lên phương án trả lời các câu hỏi khó (ví dụ: "Tại sao em code có chỗ này bị lặp?", "Bảo mật website em xử lý chỗ nào?").

## Phase 5.9: Tối Ưu Code lần cuối
- `[ ]` (Quan trọng) Mở file `AGENTS.md`, `CONVENTION.md` dọn dẹp các ghi chú nháp nếu có.
- `[ ]` Mở file `README.md`, bổ sung thông tin tài khoản Test (Admin / User thường) để BGK nếu tự test có thể đăng nhập ngay mà không cần mò mẫm đăng ký. (Cực kỳ lấy thiện cảm).
- `[ ]` Commit chốt hạ: `git commit -m "Final version for TechWiz 7 Submission"`.

## Phase 5.10: Submit / Nộp Bài
- `[ ]` Kiểm tra lại link Repo Github (nếu yêu cầu nộp link repo, nhớ add quyền hoặc chuyển thành Public).
- `[ ]` Kiểm tra URL sản phẩm live chạy ổn định.
- `[ ]` Nộp bài theo đúng form/quy định của BTC.
- `[ ]` Chúc mừng toàn team, nghỉ ngơi xả hơi và chờ kết quả!
