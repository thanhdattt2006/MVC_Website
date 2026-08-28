# HƯỚNG DẪN DÀNH CHO AI (AGENTS)

File này chứa thông tin cấu hình và hướng dẫn bắt buộc dành cho mọi AI Assistant tham gia vào dự án này.

## 1. TECH STACK (Công nghệ sử dụng)
- **Ngôn ngữ**: PHP 8.4 (Luôn sử dụng các tính năng mới nhất của PHP 8.4 như property hooks, class instantiation without extra parentheses, asymetric visibility, v.v.).
- **Framework**: Laravel (Phiên bản mới nhất, sử dụng kiến trúc MVC truyền thống).
- **Cơ sở dữ liệu**: Aiven (MySQL/PostgreSQL) - Không lưu hardcode thông tin đăng nhập trong code, chỉ dùng qua biến môi trường (.env).
- **Triển khai (Deployment)**: Render - Sử dụng `render.yaml` (Infrastructure as Code).
- **Frontend**: BẮT BUỘC sử dụng **Blade Template** kết hợp với **TailwindCSS**. TUYỆT ĐỐI không dùng CSS framework khác.

## 2. QUY TẮC CỐT LÕI (CORE RULES)
- **BẮT BUỘC HỎI Ý KIẾN TRƯỚC KHI SỬA CODE**: AI (Agent) tuyệt đối không được tự ý sửa file source code khi chưa trình bày kế hoạch và nhận được sự cho phép rõ ràng từ người dùng (Tao). Mọi đề xuất thay đổi code phải được hỏi ý kiến trước!
- **Tiêu chuẩn Git Commit**: Mọi script Git hoặc đề xuất commit message do AI tạo ra BẮT BUỘC phải bằng TIẾNG ANH, sử dụng chuẩn Conventional Commits (ví dụ: `feat: ...`, `fix: ...`).
- **Tốc độ & Hiệu quả**: Đây là dự án hackathon (TechWiz 7) có thời hạn 5 ngày. Ưu tiên code chạy được, đúng yêu cầu trước. Tối ưu hóa (optimization) thực hiện sau.
- **Không tự ý thay đổi kiến trúc**: Bám sát mô hình Model-View-Controller của Laravel.
- **Tính bảo mật**: 
  - KHÔNG BAO GIỜ hardcode mật khẩu, API keys, hoặc thông tin DB.
  - LUÔN LUÔN validate dữ liệu đầu vào (Sử dụng Form Requests của Laravel).
  - Sử dụng Eloquent ORM hoặc Query Builder, tránh viết raw SQL để chống SQL Injection.
- **Môi trường Deploy**: Render yêu cầu cấu hình web server (Apache/Nginx) qua file `render.yaml` và build script. Luôn đảm bảo script build (`composer install --no-dev`, `php artisan optimize`) hoạt động trơn tru.

## 3. CÁCH VIẾT CODE DÀNH CHO AI
- **Strict Typing & Modern PHP**: Luôn áp dụng tính năng của PHP 8.4. Luôn có Type Hinting cho Params và Return Type. Bắt buộc để `declare(strict_types=1);`.
- **No N+1 Query**: AI phải TỰ ĐỘNG nhận diện và sử dụng Eager Loading (`with()`) khi sinh code gọi data có relationship. Tuyệt đối không sinh code bị dính N+1.
- **Dependency Injection (DI)**: Ưu tiên inject các dependency class qua Constructor thay vì dùng Facade toàn cục.
- **Mã nguồn hoàn chỉnh (No Laziness)**: Code do AI sinh ra phải là code ĐẦY ĐỦ, CHẠY ĐƯỢC NGAY. CẤM viết kiểu comment `// ... logic ở đây ...`, `// ... code cũ giữ nguyên ...`.
- **Comment Code**: Viết docblock (PHPDoc) ngắn gọn cho các hàm phức tạp, giải thích TẠI SAO (Why) thay vì LÀM GÌ (What).
- **Route Definitions**: Đặt tên route rõ ràng, sử dụng Route Name (`->name('...')`).

## 4. XỬ LÝ LỖI (ERROR HANDLING)
- Trả về thông báo lỗi thân thiện với người dùng trong môi trường production (qua View hoặc JSON nếu là API).
- Log lỗi chi tiết qua hệ thống logging của Laravel (sử dụng Log facade) khi có exception xảy ra. Bất kỳ lỗi phát sinh nào đều phải được ghi vắn tắt nhưng cụ thể nguyên nhân vào file `BUGS.md`.
