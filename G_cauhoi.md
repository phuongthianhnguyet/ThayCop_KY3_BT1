### 1. Tại sao phải dùng Nginx làm Reverse Proxy mà không trỏ thẳng Tunnel vào Node-RED?
- Bảo mật & Kiểm soát: Nginx đóng vai trò như một "người gác cổng". Chị có thể cài đặt giới hạn tốc độ (Rate limiting), chặn các IP xấu, hoặc cấu hình SSL bổ sung trước khi vào đến Node-RED.

- Khả năng mở rộng: Nếu sau này chị có thêm nhiều dịch vụ khác (như Home Assistant, Web cá nhân), Nginx sẽ giúp điều hướng (Routing) dựa trên đường dẫn (ví dụ: /nodered, /web) chỉ với 1 Tunnel duy nhất.

- Xử lý tĩnh: Nginx xử lý các file tĩnh (hình ảnh, CSS) cực nhanh, giúp giảm tải cho Node-RED.

### 2. Sự khác biệt giữa việc Mount file và Mount thư mục trong Docker?
- Mount file: Chỉ gắn kết duy nhất 1 file từ máy host vào container.

Dùng khi: Chị chỉ muốn ghi đè đúng file cấu hình (như nginx.conf).

- Mount thư mục: Gắn cả một folder vào container.

Dùng khi: Chị muốn đồng bộ nhiều file cùng lúc (như thư mục chứa code website, hoặc thư mục data của Node-RED). Mọi file mới tạo ra trong thư mục ở máy host sẽ xuất hiện ngay trong container.

### 3. Nếu thay đổi file index.html ở máy Ubuntu, nội dung web có thay đổi ngay không? Tại sao?
- Có (Nếu chị dùng Mount): Do cơ chế liên kết dữ liệu trực tiếp. Container không copy file mà nó "nhìn" thẳng vào file trên máy Ubuntu của chị.

- Tại sao: Khi trình duyệt yêu cầu file, Nginx trong container sẽ đọc dữ liệu từ đường dẫn đã mount. Vì dữ liệu thực tế nằm trên ổ cứng máy Ubuntu, nên khi chị lưu file, Nginx sẽ đọc được nội dung mới ngay lập tức mà không cần khởi động lại container.

### 4. Restart: always và Restart: unless-stopped để làm gì?
- restart: always: Container sẽ tự khởi động lại trong mọi trường hợp (bị lỗi crash, máy Ubuntu bị khởi động lại, hoặc chị chủ động tắt nó đi thì nó cũng tự bật lại).

- restart: unless-stopped: Tương tự như always, nhưng có một ngoại lệ: Nếu chị chủ động gõ lệnh docker stop, nó sẽ không tự bật lại cho đến khi chị khởi động nó thủ công hoặc máy chủ khởi động lại. Đây là lựa chọn được khuyên dùng nhất.

### 5. Cách khai báo dùng chung 1 Network & Lợi ích
- Cách khai báo: Trong docker-compose.yml, tạo một mục networks ở cùng cấp với services, sau đó khai báo cho từng service.

Lợi ích:

- Bảo mật: Các dịch vụ chỉ "thấy" nhau trong nội bộ mạng đó, không lộ ra ngoài internet.

- Định danh bằng tên: Các dịch vụ gọi nhau bằng tên service (ví dụ: Nginx gọi http://nodered:1880) thay vì dùng địa chỉ IP hay thay đổi.

Sửa đổi file mẫu:
```
YAML
services:
  nginx:
    image: nginx
    networks:
      - my_network
  nodered:
    image: nodered/node-red
    networks:
      - my_network

networks:
  my_network:
    driver: bridge
```
### 6. Đưa Token vào .env và dùng .gitignore
Cách làm:
- 1. Tạo file .env ghi: CLOUDFLARE_TOKEN=your_token_here.
- 2. Trong docker-compose.yml, dùng biến ${CLOUDFLARE_TOKEN}.
- 3. Thêm dòng .env vào file .gitignore.

Tại sao quan trọng: Token là "chìa khóa" vào hệ thống của chị. Nếu chị push Token lên GitHub (đặc biệt là repo công khai), kẻ xấu có thể dùng bot quét lấy Token để chiếm quyền điều khiển domain/server của chị. Việc tách riêng giúp mã nguồn sạch và an toàn.

### 7. Tại sao nên thêm hậu tố :ro khi mount file cấu hình Nginx?
- :ro là viết tắt của Read-Only (Chỉ đọc).

- Lý do: Đảm bảo container Nginx chỉ có quyền đọc file cấu hình mà không có quyền sửa đổi nó. Điều này giúp ngăn chặn các cuộc tấn công nếu chẳng may container bị chiếm quyền (hacker không thể sửa file config để điều hướng traffic sang chỗ khác).

### 8. Khi dùng Cloudflare Tunnel: Có cần mở cổng (Open Port) không?
- Không cần thiết. Đây chính là ưu điểm lớn nhất của Tunnel.

- Cloudflare Tunnel tạo một kết nối an toàn từ bên trong máy chị đi ra ngoài. Chị không cần vào router để Port Forwarding, cũng không cần mở cổng trên tường lửa (Firewall) của Ubuntu. Điều này giúp ẩn hoàn toàn máy chủ của chị khỏi các cuộc quét cổng ẩn danh từ internet.
