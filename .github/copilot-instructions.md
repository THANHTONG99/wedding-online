# Hướng dẫn GitHub Copilot cho thiệp cưới Thanh Tòng ♥ Thu An

Repository này là website **tĩnh** xuất bản bằng GitHub Pages ở `https://thanhtong99.github.io/wedding-online/`. **Không** thêm backend, database, Supabase hoặc Render. Duy trì giao diện đỏ đô – trắng, tối ưu màn hình điện thoại và giữ nguyên thông tin cưới.

## Yêu cầu triển khai album cưới
- Trên trang chính, **chỉ** hiển thị 5 ảnh ở `#gallery` (`images/album1.jpg` đến `images/album5.jpg`).
- Bên dưới 5 ảnh là nút **“📖 CUỐN ALBUM”** (`#openAlbumBook`).
- Nút mở một **trình xem ảnh toàn màn hình điện thoại** (`#albumBook`) với 11 trang, **không cuộn dọc danh sách ảnh**. Mỗi lần chỉ hiển thị 1 trang, giữ nguyên tỷ lệ qua `object-fit:contain` (không crop ảnh).
- Khách **vuốt trái/phải** để chuyển từng trang. Có các chuyển cảnh luân phiên: trượt ngang, mờ dần, phóng to và lật trang nhẹ. Có nút mũi tên trên màn hình và hỗ trợ bàn phím trái/phải trên máy tính.
- **Khi đang ở trang cuối và vuốt tiếp sang trái hoặc nhấn nút tiếp theo, tự động đóng trình xem**, trở về đúng vị trí trong thiệp chính. Không tự đóng ngay khi mới đến trang cuối, để khách kịp xem.
- Luôn có nút **×** và hỗ trợ phím Escape để thoát. Khóa cuộn nội dung trang dưới khi album đang mở, hoàn nguyên khi đóng.
- Album có **11 trang** đúng thứ tự: `bia.webp`, `spread_001.webp` đến `spread_010.webp`, đặt tại `images/album-book/`.
- Nguồn ảnh là ZIP `album-sach-cuoi-toi-uu.zip` do chủ thiệp cung cấp. Giải nén và đưa toàn bộ **11 file .webp** vào `images/album-book/`. Không commit ZIP thay cho file ảnh.
- Giữ nguyên bố cục và chữ bên trong ảnh sách; không cắt, chỉnh sửa hoặc chép lại nội dung in trên ảnh.
- Hiệu ứng nhẹ và có hỗ trợ `prefers-reduced-motion`; ưu tiên tải trang hiện tại, tải trước trang liền kề, không tải dồn toàn bộ 11 ảnh ngay khi khách mở thiệp.
- Không xóa ảnh bìa `images/anh-cuoi.webp`, album 5 ảnh, mã VietQR, nhạc, lịch, hiệu ứng khác hay thông tin cưới.

## Cách dùng trong môi trường Copilot Agent
1. Clone/mở `THANHTONG99/wedding-online` trong VS Code.
2. Đặt ZIP `album-sach-cuoi-toi-uu.zip` vào thư mục làm việc dự án (chỉ tạm thời).
3. Giải nén ZIP vào `images/album-book/` bằng lệnh thích hợp của hệ điều hành. Ví dụ PowerShell: `Expand-Archive -LiteralPath 'album-sach-cuoi-toi-uu.zip' -DestinationPath 'images/album-book' -Force`.
4. Kiểm tra đúng 11 file `bia.webp`, `spread_001.webp`... `spread_010.webp` trong thư mục đích.
5. Kiểm tra `index.html`: `#gallery` có 5 ảnh, nút `#openAlbumBook` mở `#albumBook` và trình xem chuyển từng trang đủ 11 đường dẫn, tự thoát sau trang cuối.
6. Kiểm tra website trên máy tính và điện thoại: mở/đóng album, vuốt trái/phải, các chuyển cảnh khác nhau, vuốt tiếp ở trang cuối để thoát, không bị cuộn trang dưới và ảnh không bị cắt.
7. Chỉ commit các file web cần thiết và 11 file ảnh WebP; không commit ZIP gốc, ảnh JPG hàng trăm MB hoặc mật khẩu.

## Tiêu chí hoàn thành
- `index.html` hiện đã được điều chỉnh theo hành vi 5 ảnh + nút `CUỐN ALBUM`; thay đổi mã nguồn chỉ nếu cần sửa lỗi.
- Trên GitHub phải có `images/album-book/bia.webp` và 10 `spread_*.webp`.
- GitHub Pages hoạt động tại URL không đổi và không có ảnh lỗi 404 trong cuốn album.
