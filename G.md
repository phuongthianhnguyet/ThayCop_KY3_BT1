# G. Triển khai ứng dụng đến End-user
1. Trong Cloudflare: Tạo tunnel (đường hầm), chọn loại triển khai cho docker
2. Convert lệnh docker run ... sang dạng docker compose
3. Khai báo kết quả convert vào trong file docker-compose.yml
4. Chạy lại docker compose
5. Public ứng dụng bằng cách thêm 1 router trỏ tới container đang chạy trong docker, dữ liệu sẽ đi qua tunnel, url dạng sub-domain
6. Kiểm tra url sub-domain đã hoạt động public cho mọi end-user

# Bài làm:
## 1.  Tạo Tunnel trên Cloudflare Dashboard
Truy cập vào Zero Trust trên Cloudflare -> Networks -> Connectors -> Create a Tunnul
<img width="1908" height="1079" alt="image" src="https://github.com/user-attachments/assets/2d3ef9f4-aa87-49c2-a019-507e3ba8dd5c" />

## 2. Đặt tên cho Tunnel và nhấn save.

<img width="1899" height="1063" alt="image" src="https://github.com/user-attachments/assets/d2907187-27e6-4481-94fe-cc1ca39f9ab7" />

## 3. Chọn device's operating system là docker

<img width="1899" height="1080" alt="image" src="https://github.com/user-attachments/assets/41550857-a49a-4390-a578-dbd7b9d589d4" />

## 4. Cấu hình lại file docker-compose.yml thêm đoạn sau vào file: 

```
tunnel:
    container_name: cloudflared-tunnel
    image: cloudflare/cloudflared:latest
    restart: unless-stopped
    command: tunnel --no-autoupdate run --token <DÁN_TOKEN_CỦA_VÀO_ĐÂY>
```

- Sau khi cấu hình xong file ```docker-compose.yml``` chạy lại lệnh ```docker-compose up -d```

## 5. Quay lại cloudflare nhấn next, và đặt tên sub-domain để hoàn tất.

<img width="1917" height="1080" alt="image" src="https://github.com/user-attachments/assets/04e2c2ee-04da-49f7-ba98-e4c00468b874" />

## 6. Sau khi nhấn complete setup sẽ thấy trang Connectors hiện trạng thái healthy là đường hầm đã thông

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e4923907-2aa4-4ff5-8eef-8226606fcd2f" />

## 7. Truy cập vào web bằng tài khoản và thiết bị bất kỳ 

- Kiểm tra truy cập:

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/44f98658-53fe-4722-a090-a73bfd9bcb14" />
