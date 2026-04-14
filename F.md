# F. Gỡ lỗi
1. nếu có lỗi xẩy ra trong quá trình triển khai docker compose up -d
2. Thêm healthcheck cho myapi trong file docker-compose.yml
3. giới hạn resource cho một service: (tránh việc 1 service chiếm quá nhiều ram)
sử dụng lệnh: docker compose stats để quan sát lượng ram sử dụng bởi mỗi service

# Yêu cầu:
## 1. Kiểm tra trạng thái và xem Log
Nếu có lỗi xảy ra trong quá trình triển khai, sử dụng lệnh sau để xem:
```
docker-compose ps
```
<img width="1918" height="912" alt="image" src="https://github.com/user-attachments/assets/6361be03-ac1e-4c8b-92a3-84a49a406d9e" />

Nếu cột STATUS hiện Exit... hoặc Restarting, nghĩa là service đó đang lỗi.

## 2. Thêm Healthycheck cho myapi
Mở file ```dockẻ-compose.yml``` sửa đoạn của ```my-python-app``` như sau: 
```
my-python-app:
    build: ./myapi
    container_name: my-python-api
    ports:
      - "9630:9630"
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:9630"]
      interval: 30s    # Cứ 30 giây kiểm tra 1 lần
      timeout: 10s     # Đợi phản hồi trong 10 giây
      retries: 3       # Thử lại 3 lần nếu lỗi rồi mới báo "unhealthy"
```

## 3. Giới hạn tài nguyên
Sửa lại trong file service trong ```docker-compose.yml``` như sau: 
```
node-red:
    image: nodered/node-red:latest
    # ... các dòng khác giữ nguyên ...
    deploy:
      resources:
        limits:
          cpus: '0.50'     # Sử dụng tối đa 50% CPU
          memory: 512M    # Sử dụng tối đa 512MB RAM
```

## 4. Quan sát hiệu năng thực tế
Sau khi cấu hình và chạy lại doker, dùng lệnh dưới  đây để xem các containẻ đang dùng bao nhiêu tài nguyên
```
docker stats
```
<img width="1363" height="314" alt="image" src="https://github.com/user-attachments/assets/f9c60318-c1ba-4ab4-b3f2-4a5b099da557" />



