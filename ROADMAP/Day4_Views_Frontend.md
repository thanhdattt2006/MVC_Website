# DAY 4: FRONTEND, BLADE VIEWS VÀ GIAO DIỆN CHÍNH

**Mục tiêu**: Đắp "da thịt" lên bộ xương Logic đã làm ở Day 3. Trang web bắt buộc phải đẹp, chuyên nghiệp, responsive để gây ấn tượng với BGK TechWiz.

## Phase 4.1: Xây Dựng Master Layout (`app.blade.php`)
- `[ ]` Xác định thư mục views chính (`resources/views/layouts`).
- `[ ]` Tạo file `master.blade.php` hoặc sửa file mặc định của Breeze.
- `[ ]` Cấu trúc khung HTML5 chuẩn (Header, Main Content khu vực yield, Footer).
- `[ ]` Nhúng link CSS (Bootstrap 5 CDN hoặc Tailwind tuỳ team thống nhất). Nên dùng Tailwind (có sẵn trong Breeze) hoặc Bootstrap 5 cho nhanh.
- `[ ]` Nhúng link FontAwesome (hoặc icon libraries khác).
- `[ ]` Nhúng Google Fonts (Ví dụ: Inter, Roboto).
- `[ ]` Tách Navbar ra 1 file component riêng: `resources/views/partials/navbar.blade.php`.
- `[ ]` Tách Footer ra 1 component riêng: `resources/views/partials/footer.blade.php`.
- `[ ]` Dùng `@include('partials.navbar')` trong Layout chính.

## Phase 4.2: Tích Hợp UI Thông Báo (Flash Messages)
- `[ ]` Tạo component cho các thông báo (Success, Error). `partials/alerts.blade.php`.
- `[ ]` Viết logic check session: `@if(session('success')) <div class="alert alert-success">...</div> @endif`.
- `[ ]` Có thể tích hợp thư viện JS Toast (như Toastr hoặc SweetAlert2) để thông báo trông xịn hơn thay vì alert thuần HTML.
- `[ ]` Include phần Alert này vào vị trí đầu của thẻ Main trong Layout.

## Phase 4.3: Giao diện Trang Chủ (Home/Landing Page)
- `[ ]` Mở file view Home. Extend Layout chính (`@extends('layouts.master')`).
- `[ ]` Code Banner/Hero Section hoành tráng (Có ảnh nền, Title to, Call to Action button).
- `[ ]` Thiết kế phần hiển thị List Danh Mục (hoặc tính năng nổi bật) dạng Grid (Card).
- `[ ]` Sử dụng vòng lặp `@foreach` hiển thị các Items mới nhất (truyền từ Controller sang).
- `[ ]` Kiểm tra giao diện Mobile cho trang chủ.

## Phase 4.4: Giao diện Danh sách Items (Index)
- `[ ]` Tạo file view hiển thị list sản phẩm/bài viết.
- `[ ]` Làm form Tìm kiếm, Bộ lọc đặt ở Sidebar hoặc Topbar (Sử dụng thẻ `<form method="GET">`).
- `[ ]` Đổ dữ liệu vào giao diện dạng Danh sách (Table hoặc Grid-Card).
- `[ ]` Nếu không có data, hiển thị div "Không tìm thấy dữ liệu". (Dùng `@forelse ... @empty`).
- `[ ]` Hiển thị thanh Phân trang đẹp mắt. (Chạy `php artisan vendor:publish --tag=laravel-pagination` nếu cần sửa CSS phân trang mặc định).

## Phase 4.5: Giao diện Chi tiết (Show)
- `[ ]` Tạo layout trang chi tiết: Một bên ảnh to, một bên thông tin chi tiết.
- `[ ]` Xử lý text hiển thị an toàn (dùng `{{ $data->description }}`). Nếu là text từ trình soạn thảo mã HTML an toàn thì dùng `{!! $data->content !!}`.
- `[ ]` Thêm nút Quay lại, Nút Mua hàng / Liên hệ.
- `[ ]` Hiển thị phần "Các mục liên quan" (Related items) bên dưới trang chi tiết.

## Phase 4.6: Giao diện Form Thêm/Sửa (Create/Edit)
- `[ ]` Xây dựng form HTML với các tag `<input>`, `<select>`, `<textarea>`.
- `[ ]` Luôn chèn `@csrf` vào mọi form gửi data (POST/PUT).
- `[ ]` Bắt lỗi validation: Hiển thị `<span class="text-danger">` báo lỗi ở dưới từng ô input bằng biến `$errors->first('fieldname')` hoặc `@error('fieldname')`.
- `[ ]` (Quan trọng) Hiển thị lại dữ liệu cũ người dùng vừa nhập nếu form có lỗi: Dùng giá trị `old('fieldname', $data->fieldname ?? '')`.
- `[ ]` Style form bằng CSS framework (form-control, form-group) để form gọn gàng, đẹp mắt.

## Phase 4.7: Dashboard / Trang Quản Trị
- `[ ]` Nếu hệ thống yêu cầu quản trị viên, tạo giao diện riêng với Sidebar bên trái (AdminLTE hoặc một template dashboard đơn giản).
- `[ ]` Viết view quản lý dạng Bảng (Table). Thêm các nút Hành động: View (Mắt), Edit (Bút), Delete (Thùng rác).
- `[ ]` Nút Delete (Xóa) PHẢI là một thẻ `<form>` ẩn dùng phương thức `@method('DELETE')` và có nút Submit hoặc dùng JS (Fetch API). Không bao giờ dùng thẻ `<a>` cho route xóa để tránh crawler ấn nhầm xoá dữ liệu.

## Phase 4.8: Hiệu ứng & JavaScript (Nâng cao trải nghiệm)
- `[ ]` Viết một ít Javascript (Vanilla) hoặc jQuery để xử lý các tương tác nhỏ.
- `[ ]` Thêm xác nhận trước khi xoá (Confirm dialog) - "Bạn có chắc muốn xoá mục này không?".
- `[ ]` Thêm hiệu ứng Loading spinner (nút xoay vòng) khi bấm submit form tránh spam click nhiều lần tạo duplicate data.
- `[ ]` (Optional) Tích hợp CKEditor hoặc TinyMCE vào textarea nếu cần soạn thảo văn bản giàu định dạng.

## Phase 4.9: Tối Ưu Hình Ảnh Khuyết (Missing Image)
- `[ ]` Trong views, khi gọi ảnh, nếu record không có ảnh, phải hiển thị ảnh mặc định: `<img src="{{ $item->image_path ? asset('storage/'.$item->image_path) : asset('images/default.jpg') }}">`.
- `[ ]` Chuẩn bị sẵn file `default.jpg` đặt trong thư mục `public/images/`.

## Phase 4.10: Kiểm Tra Giao Diện Cuối Ngày
- `[ ]` F12 trình duyệt, test Responsive trên các kích thước iPhone, iPad. Sửa CSS nếu bị vỡ layout.
- `[ ]` Kiểm tra độ tương phản màu sắc.
- `[ ]` `git commit -m "Complete UI and Blade views integration"`.
- `[ ]` Push code, lên live link của Render để test trải nghiệm web thật sự ngoài môi trường mạng internet. Chú ý các đường dẫn tài nguyên (CSS/JS/Ảnh) có bị 404 không (đôi khi quên chưa symlink storage - nhưng Render thì cần lưu ý file upload sẽ bị mất sau mỗi lần deploy vì tính chất serverless, khuyến cáo dùng S3/Cloudinary cho ảnh nhưng 5 ngày có thể cân nhắc skip tùy giám khảo, hoặc dùng link URL ảnh ngoài).
