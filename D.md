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


