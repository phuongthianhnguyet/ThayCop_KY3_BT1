# C. Cấu hình docker compose
# Yêu cầu.
1. Tạo thư mục: ~/myapp
2. Chuyển vào trong thư mục ~/myapp
3. Tạo thư mục: ./myweb
4. Tạo file ./myweb/index.html (với nội dung là thông tin cá nhân của em)
5. Tạo file docker-compose.yml để nó sẽ có các dịch vụ sau:
- Khai báo sử dụng nodered/node-red, cổng 1880, dữ liệu nằm tại thư mục ./nodered
- Khai báo sử dụng nginx, cổng 80, cấu hình trong file ./nginx/nginx.conf
- Mount thư mục ./myweb thành thư mục /myweb trong nginx
- Mount file ./nginx/nginx.conf vào file /etc/nginx/nginx.conf trong nginx
6. Edit file ./nginx/nginx.conf để:
- Cấu hình web server cổng 80
- server_name là sub-domain (sub-domain tuỳ ý của em)
- location / trỏ tới root là thư mục /myweb
- location /api dùng proxy_pass trỏ tới 1 (hoặc nhiều) node http_in của nodered
7. Edit file ./nodered/settings.js để nodered bắt buộc đăng nhập
- Chạy docker-compose lần đầu để Node-RED tự sinh file cấu hình trong thư mục ./nodered, sau đó mới tiến hành sửa settings.js và restart lại container
## BÀI LÀM

#### 1. Sử dụng lệnh sau để tạo thư mục ~/myapp và chuyển vào trong thư mục đã tạo để tạo các thư mục con:

```
mkdir -p ~/myapp
cd ~/myapp
mkdir myweb nginx nodred
```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1fabc1ee-285d-4635-8aba-1b45f79adaa0" />

#### 2. Sau khi đã tạo xong thư mục rồi, tiếp tục tạo file index.html

- Tạo file ./myweb/index.html
- Sử dụng lệnh nano ~/myapp/myweb/index.html để viết nội dung file. Sau khi viết xong nội dung file, nhấn phím Ctrl+O để lưu nội dung file, và tổ hợp phím Ctrl+X để thoát nano.
```
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Profile Cá Nhân</title>
    <style>
        body {
            margin: 0;
            font-family: Arial, sans-serif;
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: #fff;
            text-align: center;
        }

        .container {
            margin-top: 100px;
        }

        .card {
            background: rgba(255,255,255,0.1);
            padding: 30px;
            border-radius: 20px;
            width: 350px;
            margin: auto;
            backdrop-filter: blur(10px);
            box-shadow: 0 8px 20px rgba(0,0,0,0.3);
        }

        img {
            width: 120px;
            height: 120px;
            border-radius: 50%;
            border: 3px solid white;
        }

        h1 {
            margin: 15px 0 5px;
        }

        p {
            opacity: 0.9;
        }

        .btn {
            display: inline-block;
            margin-top: 15px;
            padding: 10px 20px;
            background: white;
            color: #333;
            border-radius: 25px;
            text-decoration: none;
            font-weight: bold;
            transition: 0.3s;
        }

        .btn:hover {
            background: #ddd;
        }

        .social {
            margin-top: 20px;
        }

        .social a {
            color: white;
            margin: 0 10px;
            font-size: 20px;
            text-decoration: none;
        }

        .social a:hover {
            color: #ffd700;
        }
    </style>
</head>
<body>

<div class="container">
    <div class="card">
        <img src="https://i.imgur.com/6VBx3io.png" alt="avatar">

        <h1>Phương Thị Ánh Nguyệt</h1>
        <p>K225480106098 </p>
        <p>K58KTP.K01</p>

        <a href="#" class="btn">Liên hệ</a>

        <div class="social">
            <a href="#">Facebook</a>
            <a href="#">GitHub</a>
            <a href="#">Email</a>
        </div>
    </div>
</div>

</body>
</html>
```

#### 3. Tạo file docker-compose.yml
- Sử dụng lệnh nano docker-compose.yaml để tạo file docker-compose.yml
- Viết khối lệnh dưới đây vào trong file docker-compose.yml
```
version: '3.8'

services:
  node-red:
    image: nodered/node-red:latest
    container_name: node-red
    ports:
      - "1880:1880"
    volumes:
      - ./nodered:/data

  nginx:
    image: nginx:latest
    container_name: nginx-server
    ports:
      - "80:80"
    volumes:
      - ./myweb:/myweb
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - node-red
```
- Gõ lệnh ls hoặc cat docker-compose.yml để kiểm tra file docker-compose.yml vừa tạo
  
<img width="1920" height="1076" alt="image" src="https://github.com/user-attachments/assets/11316b62-0a07-4350-90da-36b8aed5ac60" />

#### 4. Cấu hình cho nginx (nginx/nginx.conf)
- Cấu hình cho nginx như sau:
```
events {}

http {
    server {
        listen 80;
        server_name nguyet_sub.local;

        location / {
            root /myweb;
            index index.html;
        }

        location /api {
            proxy_pass http://node-red:1880;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }
    }
}
```

### 5. Khởi chạy hệ thống
#### Chạy lệnh
```
docker-compose up -d
```
#### Nodered sẽ tự tạo ```file settings.js
```
Cấu hình lại file ```settings.js``` để đăng nhập:
- Mở file ```settings.js``` bỏ dấu // ở đầu đoạn ```adminAuth:{}
```
```
   adminAuth: {
        type: "credentials",
        users: [{
            username: "admin",
            password: "$2a$08$zZWtXTja0fB1pzD4sHCMyOCMYz2Z6dNbM6tl8sJogENOMcxWV9DN.",
            permissions: "*"
        }]
    },
    
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1e987f6d-5270-4f2e-b1fb-1495a82ab3b3" />

#### Sau khi cấu hình lại file settings.js, restart lại để áp dụng các thay đổi
```
docker-compose restart
```

### 6. Kiểm tra kết quả
- Truy cập Node-RED: http://192.168.126.131:1880

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b9d02e10-8793-4bce-89de-c2cb53b7ef34" />

- Truy cập web: http://192.168.126.131:80

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3bd35523-7aaa-4d99-8d52-f30338253896" />





