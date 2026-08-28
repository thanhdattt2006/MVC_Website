# DAY 2: THIẾT KẾ CƠ SỞ DỮ LIỆU, MODELS & MIGRATIONS

**Mục tiêu**: Xây dựng xong nền móng dữ liệu. Mọi thao tác đều thực hiện qua Migration, tuyệt đối không tạo bảng bằng tay bằng PhpMyAdmin/DBeaver.

## Phase 2.1: Chốt Schema Database (ERD)
- `[ ]` Sử dụng draw.io hoặc dbdiagram.io để vẽ sơ đồ.
- `[ ]` Xác định bảng Users (đã có từ Day 1). Bổ sung role (admin, user) nếu cần.
- `[ ]` Xác định Entity 1 (Ví dụ: Sản phẩm / Bài viết).
- `[ ]` Xác định Entity 2 (Ví dụ: Danh mục / Bình luận).
- `[ ]` Xác định Entity 3 (Ví dụ: Đơn hàng / Tag).
- `[ ]` Xác định Bảng trung gian cho quan hệ N-N (nếu có, vd: product_tag).
- `[ ]` Đảm bảo đã chốt đủ kiểu dữ liệu (String, Integer, Text, Boolean, Date).
- `[ ]` Gửi ảnh sơ đồ vào group chat để cả team approve.

## Phase 2.2: Tạo Migration cho Entity 1
- `[ ]` Chạy lệnh: `php artisan make:model TênEntity -m` (Cờ -m để tạo luôn file migration).
- `[ ]` Mở file migration vừa tạo trong `database/migrations`.
- `[ ]` Thêm các cột theo sơ đồ ERD. (Sử dụng `$table->string()`, `$table->integer()`, v.v.).
- `[ ]` Thiết lập Foreign Key (Khoá ngoại) nếu có. (Dùng `$table->foreignId('...')->constrained()`).
- `[ ]` Chạy `php artisan migrate` để test xem có lỗi không.
- `[ ]` Nếu lỗi, dùng `php artisan migrate:rollback`, sửa code rồi chạy lại.

## Phase 2.3: Tạo Migration cho Entity 2
- `[ ]` Lặp lại lệnh: `php artisan make:model Entity2 -m`.
- `[ ]` Code logic các cột cho Entity 2.
- `[ ]` Lưu ý thứ tự migration: Bảng nào không có khoá ngoại tạo trước, bảng chứa khoá ngoại tạo sau. (Sửa timestamp ở tên file nếu cần đổi thứ tự).
- `[ ]` Định nghĩa Foreign key tham chiếu đến bảng Users (nếu Entity2 do User tạo).
- `[ ]` Chạy test `php artisan migrate`.

## Phase 2.4: Tạo Migration cho Entity 3 & Các bảng trung gian
- `[ ]` Tạo model và migration tương tự cho Entity 3.
- `[ ]` Tạo migration riêng cho bảng trung gian (Pivot table) nếu dùng quan hệ N-N. Lệnh: `php artisan make:migration create_xxx_yyy_table`.
- `[ ]` Bảng trung gian thường chỉ gồm id của 2 bảng kia và `timestamps()`.
- `[ ]` Hoàn tất toàn bộ các bảng trong thiết kế.
- `[ ]` Chạy `php artisan migrate:fresh` để reset toàn bộ và build lại DB xem có lỗi liên kết nào không.

## Phase 2.5: Cấu hình Eloquent Models
- `[ ]` Mở file Model 1.
- `[ ]` Cấu hình thuộc tính `$fillable` (để bảo mật Mass Assignment). Liệt kê tên các cột được phép insert.
- `[ ]` Mở file Model 2, cấu hình `$fillable`.
- `[ ]` Mở file Model 3, cấu hình `$fillable`.
- `[ ]` Cấu hình thuộc tính `$hidden` nếu cần ẩn các trường nhạy cảm khi trả về JSON.
- `[ ]` (PHP 8.4) Bắt đầu áp dụng Type Hinting nghiêm ngặt nếu viết custom methods trong Model.

## Phase 2.6: Cấu hình Relationships (Quan Hệ) trong Model
- `[ ]` Định nghĩa quan hệ 1-N trong Model 1 (Ví dụ 1 Category có nhiều Product): Viết hàm `products()` dùng `return $this->hasMany(...)`.
- `[ ]` Định nghĩa quan hệ N-1 trong Model 2: Viết hàm `category()` dùng `return $this->belongsTo(...)`.
- `[ ]` Định nghĩa quan hệ N-N trong các Model liên quan: Viết hàm dùng `return $this->belongsToMany(...)`.
- `[ ]` Đảm bảo Model User cũng có hàm liên kết với các bảng mà user đó sở hữu (ví dụ `posts()`, `orders()`).
- `[ ]` Test nhanh bằng Laravel Tinker (`php artisan tinker`).

## Phase 2.7: Tạo Data Giả (Factories)
- `[ ]` Lệnh: `php artisan make:factory Model1Factory`.
- `[ ]` Mở file Factory, dùng thư viện Faker để generate data (vd: `$this->faker->name()`, `$this->faker->sentence()`).
- `[ ]` Làm tương tự tạo Factory cho Model 2.
- `[ ]` Làm tương tự tạo Factory cho Model 3.
- `[ ]` Đảm bảo Factory sinh data hợp lý, ảnh placeholder dùng các URL random.

## Phase 2.8: Tạo Seeders
- `[ ]` Mở file `database/seeders/DatabaseSeeder.php`.
- `[ ]` Comment các dòng thừa.
- `[ ]` Tạo sẵn 1 User Admin với email/pass cố định để team dùng chung (`User::factory()->create(['email' => 'admin@test.com', ...])`).
- `[ ]` Gọi Model1Factory để tạo 50 record (`Model1::factory(50)->create()`).
- `[ ]` Gọi các Factory khác. (Chú ý logic tạo dữ liệu quan hệ, phải tạo Category trước khi tạo Product).
- `[ ]` Chạy lệnh: `php artisan migrate:fresh --seed` để có một DB đầy đủ data giả.

## Phase 2.9: Commit & Deploy Data mới
- `[ ]` Kiểm tra database Aiven xem đã được dọn sạch chưa nếu là môi trường chung.
- `[ ]` Cả team commit code: `git add .`, `git commit -m "Create schemas and seeders"`.
- `[ ]` Push code lên nhánh main.
- `[ ]` Trên local team member khác, thực hiện `git pull` và chạy lệnh `php artisan migrate:fresh --seed` để đồng bộ database và code mới.
- `[ ]` Đăng nhập Render Dashboard.
- `[ ]` Chạy thủ công lệnh trên shell của Render: `php artisan migrate --force` (không seed data fake trên DB thật trừ khi thống nhất muốn có data demo).

## Phase 2.10: Tổng Kết Day 2
- `[ ]` Mọi người mở DB Client (DBeaver/Navicat/TablePlus), kết nối vào local DB kiểm tra xem các bảng đã đủ cột và data chưa.
- `[ ]` Thống nhất luồng dữ liệu cho Day 3: Controller nào phụ trách trang nào.
- `[ ]` Phân chia công việc Day 3 (Chia theo Controller/Tính năng).
