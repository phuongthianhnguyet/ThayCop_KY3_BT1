# E. Triển khai (level test) ứng dụng

1. Chuyển vào trong thư mục ~/myapp

2. Gõ lệnh để docker compose chạy: sẽ run tất cả các service khai báo trong file docker-compose.yml

  - Lợi ích: Chỉ cần docker-compose up -d là toàn bộ hệ thống (Web + Node-RED + Tunnel) tự chạy,

3. Kiểm tra các container đang chạy trong docker, nếu có cái nào bị restart cần tìm lỗi rồi edit lại docker-compose.yml

4. Kiểm tra kiểm thử các service đang chạy độc lập thông qua ip và port của nó: ví dụ mở trình duyệt ip_ubuntu:1880 để check nodered đã chạy chưa

5. Sử dụng nodered: kéo nodered http_in , http_response, function : để tạo api get đơn giản (dùng cho /api proxy_pass của nginx)

6. Sửa file ./myweb/index.html : thêm code html+js để sử dụng được api đã khai báo proxy_pass (thực ra là sử dụng nodered http_in hoặc sử dụng service myapi) 

---------------------
# Bài làm
#### 1. Khởi chạy hệ thống
- Chuyển vào thư mục ~myapp
```
cd myapp
```
- Chạy docker compose để run tất cả các service đã khai báo trong file docker-compose.yml
```
docker-compose up -d
```
- Sau khi chạy lệnh này, toàn bộ hệ thống Web, Node-Red, Myapi tự chạy.
#### 2. Kiểm tra containẻ đang chạy trong docker
- Dùng lệnh docker-compose ps để kiểm tra trạng thái các container đang chạy trong docker
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c42d1e1d-a9fc-4d40-ac88-b1605e57c737" />
#### 3. Kiểm thử
Truy cập 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/576361c5-f147-488a-bbb9-648b6d6bcaa9" />
Truy cập 
<img width="1915" height="1080" alt="image" src="https://github.com/user-attachments/assets/ea540a98-cdd7-40b0-bf85-753aa4d5e3db" />
truy cập 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6a547c10-14ac-4f66-8f09-eb3b7763003d" />
Tất cả đều chạy ok
#### 4. Tạo API đơn giản trên Node-RED
<img width="1919" height="1080" alt="image" src="https://github.com/user-attachments/assets/81db45c9-f308-49dd-9464-cd24fa03f22e" />


#### 5. Sửa file
