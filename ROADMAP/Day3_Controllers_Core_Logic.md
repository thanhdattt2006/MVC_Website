# DAY 3: CORE LOGIC, CONTROLLERS VÀ ROUTING

**Mục tiêu**: Xây dựng xương sống của ứng dụng. Nhận request, lấy dữ liệu, xử lý nghiệp vụ và ném sang View. Tạm thời chưa cần View đẹp, dùng `dd()` hoặc ném ra mảng JSON để test logic.

## Phase 3.1: Quy hoạch Route File (`web.php`)
- `[ ]` Xóa/Comment các route mặc định không cần thiết.
- `[ ]` Nhóm các route yêu cầu Đăng Nhập vào `Route::middleware(['auth'])->group(...)`.
- `[ ]` Tạo các route public (Guest cũng xem được) cho trang chủ, danh sách, chi tiết.
- `[ ]` Sử dụng chuẩn RESTful Resource Route nếu có thể: `Route::resource('products', ProductController::class)`.
- `[ ]` Đặt tên cho TẤT CẢ các route thủ công (VD: `->name('home')`, `->name('product.detail')`).
- `[ ]` Chạy lệnh `php artisan route:list` để kiểm tra toàn bộ URL ứng dụng xem có bị trùng/lỗi không.

## Phase 3.2: Khởi tạo Controllers
- `[ ]` Người A: Tạo Controller cho Entity 1 (VD: `php artisan make:controller ProductController`).
- `[ ]` Người B: Tạo Controller cho Entity 2.
- `[ ]` Người C: Tạo Controller cho Entity 3.
- `[ ]` Người D: Tạo Controller cho User Profile / Dashboard.
- `[ ]` Add các method chuẩn của CRUD vào controller (index, create, store, show, edit, update, destroy).

## Phase 3.3: Implement Logic - Danh sách (Index) & Chi tiết (Show)
- `[ ]` Controller 1 - `index()`: Dùng Eloquent để lấy danh sách data. Áp dụng Phân trang (`paginate(15)`).
- `[ ]` Controller 1 - `show()`: Lấy data theo ID (`findOrFail($id)`). Eager Load các quan hệ nếu cần (VD: `$product->load('category', 'tags')`).
- `[ ]` Test bằng cách `return response()->json($data)` tạm thời để xem dữ liệu có xuất ra đúng trên trình duyệt không.
- `[ ]` Các Controller khác lặp lại quy trình trên cho method `index` và `show`.

## Phase 3.4: Form Validation (Tạo Form Requests)
- `[ ]` **Quy tắc DRY**: KHÔNG dùng `$request->validate()` lộn xộn trong Controller.
- `[ ]` Tạo Request Class: `php artisan make:request StoreEntity1Request`.
- `[ ]` Trong file Request, sửa `authorize()` trả về `true` (Hoặc kiểm tra quyền sở hữu tại đây).
- `[ ]` Viết rules validate trong `rules()`. Ví dụ: `'name' => 'required|string|max:255'`.
- `[ ]` Tạo `UpdateEntity1Request` (Thường rules sẽ lỏng hơn Store một chút, vd bỏ `required` cho file ảnh).
- `[ ]` Lặp lại tạo Form Request cho tất cả các form (Thêm, Sửa) của mọi Entity.

## Phase 3.5: Implement Logic - Tạo mới (Store)
- `[ ]` Inject FormRequest vừa tạo vào method `store(StoreRequest $request)`.
- `[ ]` Lấy dữ liệu an toàn từ request (`$validated = $request->validated()`).
- `[ ]` Xử lý upload File/Image nếu có. Lưu file vào storage/public và lấy path gán vào data.
- `[ ]` Lưu vào DB: `Model::create($validated)`.
- `[ ]` Xử lý quan hệ N-N (nếu có, dùng hàm `sync()`).
- `[ ]` Redirect người dùng về trang danh sách kèm Flash Session báo thành công (`->with('success', 'Đã thêm!')`).

## Phase 3.6: Implement Logic - Cập nhật (Update) & Xoá (Destroy)
- `[ ]` Method `update()`: Inject `UpdateRequest`. Tìm record bằng `findOrFail($id)`.
- `[ ]` Cập nhật dữ liệu từ `$request->validated()`.
- `[ ]` Xử lý xóa ảnh cũ nếu có upload ảnh mới thay thế (để tối ưu dung lượng Render).
- `[ ]` `save()` record. Redirect báo thành công.
- `[ ]` Method `destroy()`: Tìm record. Kiểm tra quyền (VD: user đang đăng nhập có phải là người tạo ra nó không).
- `[ ]` Chạy lệnh `$model->delete()`. Xóa luôn ảnh đính kèm (nếu logic yêu cầu).
- `[ ]` Redirect báo thành công.

## Phase 3.7: Implement Advanced Query (Lọc & Tìm Kiếm)
- `[ ]` Trong phương thức `index()`, thêm logic bắt param trên URL (VD: `?search=abc&category_id=1`).
- `[ ]` Dùng Query Builder `when()` của Laravel để viết điều kiện tìm kiếm động: `$query->when($search, function($q)...)`.
- `[ ]` Không được viết logic tìm kiếm kiểu nối chuỗi SQL rườm rà (Chống Injection).
- `[ ]` Cập nhật trả data sau khi query về dạng pagination.

## Phase 3.8: Xử lý Authorization (Phân quyền)
- `[ ]` Nếu hệ thống có Role (Admin/User), tạo 1 Middleware `CheckAdmin`.
- `[ ]` Lệnh: `php artisan make:middleware IsAdmin`.
- `[ ]` Viết logic check `$request->user()->role == 'admin'` trong Middleware.
- `[ ]` Đăng ký Middleware vào `bootstrap/app.php` (tùy chuẩn cấu trúc Laravel 11/10) hoặc gọi thẳng trong web.php.
- `[ ]` Áp dụng Middleware cho các route chỉnh sửa/xóa hoặc trang quản trị.

## Phase 3.9: Kiểm Tra Chéo (Cross-Testing)
- `[ ]` Người A dùng Postman (hoặc trình duyệt test JSON) test các route của Người B.
- `[ ]` Test các case nhập thiếu dữ liệu xem FormRequest có chặn đúng không.
- `[ ]` Test xóa một record không tồn tại xem có văng lỗi 404 chuẩn của Laravel không.
- `[ ]` Nếu xảy ra Exception trắng trang, phải bắt try-catch và return message tử tế.
- `[ ]` Sửa ngay các bug tìm thấy.

## Phase 3.10: Tổng Kết Day 3 & Push Code
- `[ ]` Xóa sạch các `dd()`, `dump()` trong code.
- `[ ]` `git add .`, `git commit -m "Implement core controllers and logic"`.
- `[ ]` Push nhánh.
- `[ ]` Xác nhận Deploy Render không bị lỗi do thiếu class hoặc sai tên hàm.
- `[ ]` Gán task làm giao diện (Views) cho Day 4.
