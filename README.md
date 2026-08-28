# TechWiz 7 Project - Laravel MVC

Đây là repository cho dự án TechWiz 7. 
Thời gian phát triển: 5 Ngày.
Công nghệ: PHP 8.4, Laravel MVC, Aiven DB, Render.

## 🚀 Hướng Dẫn Cài Đặt (Cho Team Member)

Mỗi người có 4h/ngày, hãy làm đúng theo các bước này để không mất thời gian setup:

### 1. Clone Code
```bash
git clone <url-repo-cua-ban>
cd Laravel_MVC
```

### 2. Cài Đặt Dependencies (PHP & Node)
```bash
composer install
npm install
```

### 3. Cấu hình Môi Trường
- Copy file `.env.example` thành `.env`:
```bash
cp .env.example .env
```
- Mở file `.env` và điền thông tin Database Aiven mà team leader đã cung cấp (Mục `DB_HOST`, `DB_PORT`, `DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD`).
- Chạy lệnh sinh Key:
```bash
php artisan key:generate
```

### 4. Chạy Migration (Tạo Bảng trong DB)
- **Cảnh báo:** Hãy chắc chắn database đã kết nối được.
```bash
php artisan migrate
```
- Nếu cần chạy seed (data mẫu): `php artisan migrate --seed`

### 5. Khởi Động Server
Mở 2 terminal:
- Terminal 1 (Chạy Laravel):
```bash
php artisan serve
```
- Terminal 2 (Biên dịch Frontend Vite/Mix):
```bash
npm run dev
```

Truy cập: `http://localhost:8000`

## 📚 Tài liệu Nội Bộ
Hãy đọc kỹ các file sau trước khi code:
1. `CONVENTION.md`: Quy tắc đặt tên, code sạch.
2. `ROADMAP/`: Thư mục chứa tiến độ. Ai rảnh vào lấy task tick `[x]` nhé.
