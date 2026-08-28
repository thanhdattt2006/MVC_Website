# QUY TẮC LẬP TRÌNH (CODING CONVENTIONS)

Dự án này áp dụng các quy chuẩn chặt chẽ để đảm bảo code sạch, dễ bảo trì, giúp team 4 người có thể phối hợp nhịp nhàng trong thời gian 5 ngày ngắn ngủi (mỗi người 4h/ngày).

## 1. NGUYÊN TẮC THIẾT KẾ (DESIGN PRINCIPLES)

### 1.1. S.O.L.I.D Principles
- **S - Single Responsibility**: Mỗi Class/Method chỉ làm duy nhất MỘT việc. (Ví dụ: Controller chỉ xử lý request/response, logic tính toán phức tạp chuyển vào Service, truy vấn DB phức tạp chuyển vào Repository/Model).
- **O - Open/Closed**: Code có thể mở rộng nhưng hạn chế sửa đổi code cũ.
- **L - Liskov Substitution**: Class con phải thay thế được class cha mà không làm hỏng chương trình.
- **I - Interface Segregation**: Không bắt class implement những interface mà nó không dùng đến.
- **D - Dependency Inversion**: Phụ thuộc vào abstraction (interface/abstract class), không phụ thuộc vào implementation cụ thể. (Tích cực dùng Dependency Injection của Laravel).

### 1.2. D.R.Y (Don't Repeat Yourself)
- TUYỆT ĐỐI không copy-paste code lặp đi lặp lại.
- Trích xuất code dùng chung ra các Helpers, Traits, Base Classes, hoặc View Components.

### 1.3. K.I.S.S (Keep It Simple, Stupid)
- Giữ code đơn giản nhất có thể. Đừng "over-engineer" (viết quá phức tạp) cho những chức năng nhỏ. (Nhắc nhở: Thời gian chỉ có 5 ngày!).

### 1.4. ĐIỀU KIỆN RÀNG BUỘC CỨNG (STRICT CONSTRAINTS)
- **Strict Types**: BẮT BUỘC thêm dòng `declare(strict_types=1);` ở đầu TẤT CẢ các file PHP (Controller, Model, Service...).
- **Type Hinting**: BẮT BUỘC khai báo kiểu dữ liệu cho toàn bộ param và return type của hàm. Sử dụng Union Types, Intersection Types của PHP 8. Không có return type -> `void`.
- **Superglobals & Helpers**: TUYỆT ĐỐI không dùng `$_GET`, `$_POST`, `$_REQUEST`. CẤM dùng hàm `env()` trong source code logic (hàm `env()` chỉ được gọi ở thư mục `config/`, những chỗ khác bắt buộc gọi qua hàm `config()`).
- **Magic Numbers/Strings**: CẤM hardcode số hay chuỗi vô nghĩa vào logic (Ví dụ: `if($status == 1)`). BẮT BUỘC định nghĩa thành hằng số trong Model (Ví dụ: `const STATUS_ACTIVE = 1;`).
- **Chống N+1 Query**: TUYỆT ĐỐI KHÔNG thực hiện query CSDL trong vòng lặp (cả trong PHP script lẫn trong view Blade `@foreach`). BẮT BUỘC sử dụng Eager Loading `with()` khi gọi relationship. Đứa nào vi phạm -> bắt code lại toàn bộ.
- **Dữ liệu**: Các bảng dữ liệu quan trọng (User, Order, Product...) cấm xoá vật lý. Bắt buộc dùng trait `SoftDeletes` của Laravel.

## 2. QUY CHUẨN ĐẶT TÊN (NAMING CONVENTIONS)

- **Controllers**: `PascalCase` và kết thúc bằng chữ Controller (VD: `UserController`, `OrderController`). Luôn dùng số ít.
- **Models**: `PascalCase` số ít (VD: `User`, `Product`).
- **Database Tables**: `snake_case` số nhiều (VD: `users`, `product_categories`).
- **Migrations**: `snake_case` (VD: `2023_01_01_000000_create_users_table.php`).
- **Methods/Functions**: `camelCase` (VD: `getUserById()`, `calculateTotal()`).
- **Variables**: `camelCase` (VD: `$userName`, `$totalPrice`).
- **Constants**: `UPPER_SNAKE_CASE` (VD: `MAX_UPLOAD_SIZE`, `STATUS_ACTIVE`).
- **Views**: `kebab-case` (VD: `user-profile.blade.php`, `create-order.blade.php`).

## 3. CẤU TRÚC THƯ MỤC VÀ LUỒNG DỮ LIỆU
1. **Route**: Tiếp nhận request, gọi đến Controller tương ứng. Không viết logic ở Route!
2. **Middleware**: Xử lý logic trước/sau khi vào Controller (Auth, Logging, Role checking).
3. **Form Request**: Validate dữ liệu đầu vào. (Ví dụ: `StoreUserRequest`). KHÔNG validate trực tiếp trong Controller.
4. **Controller**: Nhận request đã validate, gọi Service/Model xử lý, trả về View hoặc Response. Giữ controller càng "gầy" (Thin Controller) càng tốt. TỐI ĐA không quá 50 dòng code/phương thức.
5. **Model/Service**: Xử lý "Fat Logic" (Logic nghiệp vụ nặng) và tương tác Database.
6. **View**: BẮT BUỘC sử dụng TailwindCSS cho giao diện. Phải bóc tách thành các Component nhỏ (`resources/views/components/`) để tái sử dụng tối đa code frontend.

## 4. QUY TRÌNH GIT VÀ COMMITS
- **Branching**:
  - `main`: Code ổn định, sẵn sàng deploy lên Render.
  - `dev`: Code đang phát triển tích hợp.
  - `feature/tên-chức-năng`: Nhánh làm chức năng mới (VD: `feature/login`, `feature/cart`).
- **Commit Messages (BẮT BUỘC TIẾNG ANH)**: 
  - Tuân thủ chuẩn **Conventional Commits** (`type: message`). Không viết hoa chữ cái đầu tiên của loại commit.
  - Các loại commit được phép (LIỆT KÊ ĐẦY ĐỦ, KHÔNG CHẾ THÊM): 
    - `feat` (tính năng mới)
    - `fix` (sửa lỗi)
    - `docs` (tài liệu)
    - `style` (format code, khoảng trắng...)
    - `refactor` (tối ưu code, sạch code)
    - `perf` (tăng hiệu năng)
    - `test` (thêm/sửa test)
    - `build` (cấu hình build, dependencies)
    - `ci` (cấu hình CI/CD, deploy)
    - `chore` (việc vặt, cấu hình linh tinh)
    - `revert` (hoàn tác commit cũ)
  - Ví dụ đúng: `feat: add user authentication`, `fix: correct calculation in cart`.
  - **TRẢM LIÊN TỤC**: Xong 1 tính năng nhỏ là phải commit ngay lập tức. Tuyệt đối KHÔNG ĐƯỢC dồn cả đống code rồi mới commit 1 lần.

## 5. BẮT BUỘC (MUST-HAVE) KHI VIẾT CODE
1. Luôn bắt exception `try-catch` ở những chỗ tương tác với service bên ngoài hoặc database phức tạp.
2. Code phải pass lệnh kiểm tra cú pháp trước khi push.
3. Không push file `.env` lên Github (đã có trong `.gitignore`).
