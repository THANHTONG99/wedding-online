# Wedding Cloud – Thiệp cưới cá nhân

Ứng dụng full-stack tiếng Việt gồm website thiệp cưới, trang quản trị `/admin`, Express API, PostgreSQL, RSVP, lời chúc chờ duyệt, tải ảnh Cloudinary và xuất danh sách khách mời CSV.

## Yêu cầu
- Node.js >=20, npm
- PostgreSQL 15+ (local qua Docker hoặc cloud)
- Cloudinary (tùy chọn, để tải ảnh từ trang quản trị)

## Chạy trên máy tính
1. Cài Node.js và Docker Desktop.
2. Chạy `docker compose up -d` trong thư mục dự án.
3. Sao chép `.env.example` thành `.env` và đổi `ADMIN_EMAIL`, `ADMIN_PASSWORD` (ít nhất 12 ký tự), `SESSION_SECRET` (chuỗi ngẫu nhiên dài).
4. Chạy `npm install`.
5. Chạy `npm run db:init` để tạo bảng và tài khoản quản trị. Lệnh này cập nhật mật khẩu admin từ `.env` khi chạy lại.
6. Chạy `npm start` hoặc `npm run dev`.
7. Truy cập `http://localhost:3000` (thiệp) và `http://localhost:3000/admin` (quản trị).

## Deploy lên Render.com
1. Đưa thư mục dự án lên GitHub (KHÔNG commit `.env`).
2. Tạo PostgreSQL database trên Render hoặc nhà cung cấp PostgreSQL được quản lý. Lấy `DATABASE_URL` (ưu tiên Internal Database URL nếu app và DB cùng Render).
3. Tạo **Web Service** từ repo GitHub, Runtime `Node`, Build Command `npm install`, Start Command `npm start`, Health Check Path `/health`.
4. Thiết lập environment variables: `NODE_ENV=production`, `DATABASE_URL`, `ADMIN_EMAIL`, `ADMIN_PASSWORD`, `SESSION_SECRET` và `PUBLIC_URL=https://ten-app.onrender.com`. Nếu nhà cung cấp DB yêu cầu TLS, đặt `DATABASE_SSL=true` (xem lưu ý TLS bên dưới).
5. Trong Render Shell hoặc máy local có quyền kết nối tới database, chạy `npm run db:init` **một lần trước khi truy cập**. Có thể đặt Render Pre-Deploy Command là `npm run db:init` nếu gói dịch vụ hỗ trợ, nhưng lưu ý chạy lại sẽ đồng bộ mật khẩu admin theo biến môi trường.
6. Mở `https://ten-app.onrender.com/admin` để chỉnh sửa nội dung và công khai thiệp.
7. Tùy chọn: kết nối custom domain và HTTPS theo hướng dẫn Render.

**Lưu ý TLS:** `DATABASE_SSL=true` trong bản mẫu cho phép kết nối TLS với `rejectUnauthorized:false` nhằm hỗ trợ một số dịch vụ managed DB; cấu hình này **không xác thực chứng chỉ server**. Với môi trường production yêu cầu bảo mật cao, hãy sửa `server/db.js` để xác thực CA của nhà cung cấp DB, hoặc dùng kết nối nội bộ tin cậy theo tài liệu nhà cung cấp.

## Tải ảnh
Tạo tài khoản Cloudinary, thêm `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET` vào environment. Trên `/admin`, chọn ảnh JPG/PNG/WEBP dưới 5MB và nhấn **Tải ảnh lên Cloudinary**. URL được thêm vào album; nhấn **Lưu thông tin** để xuất bản. Ảnh bìa có thể dùng URL HTTPS của ảnh đã tải.

## API
- `GET /api/settings` – nội dung thiệp công khai (chỉ khi `published=true`)
- `POST /api/rsvp` – gửi xác nhận tham dự
- `GET /api/wishes` – lời chúc đã duyệt
- `POST /api/wishes` – gửi lời chúc chờ duyệt
- `POST /api/admin/login`, `POST /api/admin/logout`, `GET /api/admin/me`
- `GET/PUT /api/admin/settings` – đọc/sửa nội dung
- `GET /api/admin/rsvps` – danh sách RSVP
- `GET /api/admin/wishes`, `PATCH/DELETE /api/admin/wishes/:id` – quản lý lời chúc
- `POST /api/admin/upload` – tải ảnh Cloudinary

## Bảo mật và vận hành
- Mật khẩu admin được băm bcrypt; session ngẫu nhiên lưu hash trong PostgreSQL, cookie HttpOnly/SameSite.
- API ghi dữ liệu có rate limit cơ bản, SQL dùng tham số, dữ liệu lời chúc hiển thị bằng `textContent` tránh HTML injection.
- Sao lưu PostgreSQL định kỳ; bật 2FA ở GitHub, Render và Cloudinary; đặt secrets qua dashboard, không commit vào git.
- Bản mẫu dành cho **một thiệp cưới cá nhân**, chưa có captcha, email/SMS, thanh toán, đa ngôn ngữ, phân quyền nhiều admin hoặc hệ thống giám sát nâng cao.
- `helmet` được bật nhưng CSP bị tắt để đơn giản hóa giao diện; trước khi vận hành quy mô lớn nên thiết lập CSP chặt chẽ và thêm kiểm thử, logging, CSRF defense theo yêu cầu.
- URL nhạc phải là file có thể phát trên trình duyệt; nhạc chỉ phát sau khi khách bấm nút do hạn chế autoplay.
