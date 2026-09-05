# Laravel MVC TechWiz 7 Project

Dự án này được xây dựng dựa trên framework **Laravel 11** (hoặc mới nhất) kết hợp với kiến trúc **MVC truyền thống** và **TailwindCSS**. 
Dưới đây là hướng dẫn chi tiết dành cho các thành viên trong team để clone code về máy và setup môi trường chạy chuẩn xác nhất.

## 🚀 Hướng Dẫn Cài Đặt (Dành Cho Team)

### Bước 1: Clone mã nguồn về máy
Mở Terminal (hoặc Git Bash) tại thư mục bạn muốn chứa code và chạy lệnh:
```bash
git clone <địa-chỉ-repo-github-của-team>
cd Laravel_MVC
```
*(Lưu ý: Thay `<địa-chỉ-repo-github-của-team>` bằng URL thật của repo dự án).*

### Bước 2: Cài đặt thư viện PHP (Composer)
Dự án sử dụng Composer để quản lý các package backend. Chạy lệnh:
```bash
composer install
```

### Bước 3: Cài đặt thư viện Frontend (NPM) và Build Tailwind
Dự án sử dụng **Vite** và **TailwindCSS**. Bắt buộc phải cài đặt node_modules và build frontend thì giao diện mới hiển thị được (nếu không sẽ bị lỗi 500 Vite Manifest Not Found).
```bash
npm install
npm run build
```
*(Mẹo: Khi code giao diện, bạn có thể chạy `npm run dev` ở một terminal riêng biệt để code tự động update (Hot Reload) mà không cần f5).*

### Bước 4: Cấu hình file môi trường (.env)
Copy file `.env.example` thành file `.env` chứa các config cho máy local của bạn:
- **Windows (Command Prompt):** `copy .env.example .env`
- **Mac/Linux/PowerShell:** `cp .env.example .env`

Sau đó mở file `.env` lên và cấu hình lại phần Database. 
*Nếu team đang xài Database chung trên Aiven:*
```env
DB_CONNECTION=mysql
DB_HOST=<Aiven_Host_Của_Team>
DB_PORT=<Aiven_Port>
DB_DATABASE=<Tên_DB>
DB_USERNAME=<User_Name>
DB_PASSWORD=<Mật_khẩu>
```
*Nếu bạn chạy Database local (XAMPP/Laragon):*
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=
```

### Bước 5: Tạo App Key
Laravel cần key này để mã hóa session và dữ liệu. Chạy lệnh:
```bash
php artisan key:generate
```

### Bước 6: Chạy Migration (Tạo bảng CSDL)
Nếu bạn xài DB Local (hoặc DB chung nhưng chưa có bảng), hãy chạy lệnh này để Laravel tạo cấu trúc bảng:
```bash
php artisan migrate
```

### Bước 7: Khởi động Server
Chạy lệnh sau để bật server ảo của Laravel:
```bash
php artisan serve
```
Mở trình duyệt và truy cập: `http://localhost:8000/demo` để kiểm tra thành quả!

---
## 📝 Quy tắc làm việc (Must Read)
- Trước khi code, vui lòng đọc kỹ các luật trong file `RULE.md` và `ai/CONVENTION.md`.
- File theo dõi tiến độ nằm ở `ai/PROGRESS.md`.
- Bất kỳ lỗi khó nào phát sinh, hãy log vào `ai/BUGS.md`.
