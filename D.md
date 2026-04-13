# D. Bonus
# Yêu cầu: 
1. Tạo thư mục ./myapi
2. Tạo file ./myapi/app.py sử dụng Python + Flask để làm gì đó funny
3. Tạo file ./myapi/requirements.txt chứa các thư viện mà app.py sử dụng (theo như app.py ví dụ thì requirements.txt chỉ cần có nội dung: flask)
4. Tạo file ./myapi/Dockerfile để khai báo sử dụng Python 3.9 slim
5. Sửa đổi docker-compose để sử dụng myapp (xem phần tham khảo ở dưới)
6. Sửa đổi nginx/nginx.conf để /api trỏ tới service myapp cổng 9630
----------
# Bài làm: 
## 1. Tạo thư mục ```myapi``` và file ```app.py```, ```requirements.txt```, ```Dockerfile```
Lệnh tạo: 
```
mkdir myapi
nano myapi/app.py
nano requirements.txt
nano Dockerfile
```

Nội dung file ```app.py```:
```
from flask import Flask, jsonify
import random

app = Flask(__name__)

@app.route('/api')
def funny_api():
    jokes = [
        "Tại sao lập trình viên thích chế độ tối? Vì ánh sáng thu hút bọ (bugs)!",
        "Lập trình viên là một sinh vật có khả năng biến caffeine thành mã nguồn.",
        "Sáng tạo là khi bạn viết code mà không cần Stack Overflow.",
        "Gõ xong code mà không lỗi? Chắc chắn có gì đó sai sai ở đây rồi!"
    ]
    return jsonify({
        "status": "success",
        "message": random.choice(jokes),
        "author": "Nguyet's Bot"
    })

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=9630)
```

Nội dung file ```requirements.txt```:
```
flask
```
Nội dung file ```Dockerfile```:
```
 # Sử dụng phiên bản Python nhẹ (alpine) để giảm dung lượng image
 FROM python:3.9-slim

 # Thiết lập thư mục làm việc bên trong container
 WORKDIR /app

 # Sao chép file requirements vào và cài đặt thư viện
 COPY requirements.txt .
 RUN pip install --no-cache-dir -r requirements.txt

 # Sao chép toàn bộ mã nguồn vào container
 COPY . .

 # Thông báo container sẽ chạy ở cổng 9630
 EXPOSE 9630

 # Lệnh khởi chạy ứng dụng
 CMD ["python", "app.py"]
```

<img width="1425" height="878" alt="image" src="https://github.com/user-attachments/assets/4d13793a-9aad-4979-b3da-b7438ce4d47c" />

## 2. Cấu hình lại file ```docker-compose.yml```
Thêm dịch vụ ```myapi```
```
 my-python-app:
    build: ./myapi
    container_name: my-python-api
```

## 3. Cấu hình lại ```nginx.conf```
Sửa dòng ```proxy_pass``` dưới đây:
```
  location /api {
            proxy_pass http://my-python-app:9630;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }
```

Sau hi cấu hình lại file ```docker-compose.yml``` và ```nginx.conf``` thì chạy lệnh sau để build lại thư mục myapi
```
docker-compose up -d --build
```

Truy cập các địa chỉ sau để kiểm tra: 
- Truy câp: http://192.168.126.131:9630
<img width="1913" height="1080" alt="image" src="https://github.com/user-attachments/assets/ad9fb376-128b-4cf6-9e03-f2993560c9ba" />

- Truy cập: http://192.168.126.131/api/
<img width="1915" height="1080" alt="image" src="https://github.com/user-attachments/assets/c835ba0f-1024-4cbe-812b-91fc0aa9b1fb" />

- Truy cập: http://192.168.126.131/
<img width="1918" height="1080" alt="image" src="https://github.com/user-attachments/assets/5bab9a58-9680-4e89-b078-1e79397230b6" />

