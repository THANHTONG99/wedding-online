# Hướng dẫn GitHub Copilot cho thiệp cưới Thanh Tòng ♥ Thu An

Repository này là website **tĩnh** xuất bản bằng GitHub Pages ở `https://thanhtong99.github.io/wedding-online/`. **Không** thêm backend, database, Supabase hoặc Render. Duy trì giao diện đỏ đô – trắng, tối ưu màn hình điện thoại và giữ nguyên thông tin cưới.

## Yêu cầu triển khai album cưới
- Trên trang chính, **chỉ** hiển thị 5 ảnh ở `#gallery` (`images/album1.jpg` đến `images/album5.jpg`). Không đưa 11 trang của sách cưới trực tiếp lên trang chính.
- Bên dưới 5 ảnh có nút ghi chính xác **“📖 CUỐN ALBUM”** (`#openAlbumBook`).
- Khi nhấn, mở một lớp phủ toàn màn hình `#albumBook` có vùng **cuộn dọc**, nút đóng, không làm thay đổi URL. Trên điện thoại phải cuộn mượt, ảnh giữ **nguyên tỷ lệ trang đôi**, không crop, không làm vỡ bố cục chữ đã in trong album.
- Album sách có **11 trang** đúng thứ tự: `bia.webp`, `spread_001.webp` đến `spread_010.webp`, đặt tại `images/album-book/`.
- Nguồn ảnh là tập tin ZIP do chủ thiệp cung cấp có tên `album-sach-cuoi-toi-uu.zip`. Sau khi giải nén, đưa toàn bộ **11 file .webp** vào `images/album-book/`. Không commit file ZIP thay cho các file ảnh; không yêu cầu trình duyệt giải nén ZIP.
- Tải ảnh sách theo nhu cầu và giữ `loading="lazy"` để tiết kiệm dữ liệu.
- Chữ trong ảnh sách là một phần của ảnh; không chép lại hoặc thay đổi tên/ngày cưới trong các trang đã in.
- Không xóa ảnh bìa hiện tại `images/anh-cuoi.webp`, album 5 ảnh, mã VietQR, nhạc, lịch, hiệu ứng hoặc các tính năng hiện có.

## Cách dùng trong môi trường Copilot Agent
1. Clone/mở `THANHTONG99/wedding-online` trong VS Code.
2. Đặt ZIP `album-sach-cuoi-toi-uu.zip` vào thư mục làm việc dự án (chỉ tạm thời).
3. Giải nén ZIP vào `images/album-book/` bằng lệnh thích hợp của hệ điều hành. Ví dụ PowerShell: `Expand-Archive -LiteralPath 'album-sach-cuoi-toi-uu.zip' -DestinationPath 'images/album-book' -Force`.
4. Kiểm tra đúng 11 file `bia.webp`, `spread_001.webp`... `spread_010.webp` trong thư mục đích.
5. Kiểm tra `index.html`: `#gallery` có 5 ảnh, nút `#openAlbumBook` mở `#albumBook` và nội dung viewer tham chiếu đủ 11 đường dẫn.
6. Kiểm tra website trên máy tính và điện thoại: mở/đóng album, vuốt cuộn, không bị cuộn trang dưới khi album mở, không tràn chiều ngang, ảnh không bị cắt.
7. Chỉ commit các file web cần thiết và 11 file ảnh WebP; không commit ZIP gốc, ảnh JPG hàng trăm MB hoặc mật khẩu.

## Tiêu chí hoàn thành
- `index.html` hiện đã được điều chỉnh theo hành vi 5 ảnh + nút `CUỐN ALBUM`; thay đổi mã nguồn chỉ nếu cần sửa lỗi.
- Trên GitHub phải có `images/album-book/bia.webp` và 10 `spread_*.webp`.
- GitHub Pages hoạt động tại URL không đổi và không có ảnh lỗi 404 trong cuốn album.
