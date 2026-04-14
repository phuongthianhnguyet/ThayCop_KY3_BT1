# B. Cài đặt Ubuntu + Docker
# Yêu cầu:
1. Cài đặt hệ điều hành Ubuntu 24.04.4 LTS
- Sử dụng một trong các công cụ để giả lập: HyperV (có sẵn của windows), VirutualBox (Miễn phí), VM_Ware (Bản quyền).
- Download file iso để cài đặt.
Cấu hình mạng trong Ubuntu (và công cụ để giả lập) để cho phép truy cập SSH vào Ubuntu từ cmd của windows.
2. Tìm hiểu các lệnh cơ bản của Ubuntu
Các lệnh cần tìm hiểu:

- Liệt kê các file trong thư mục: ls
- Tạo thư mục: mkdir nameFolder
- Chuyển thư mục làm việc: cd path
- Copy file: cp file_nguồn path/file_đích
- Thay đổi quyền thao tác file: sudo chmod xxx filename
- Edit file: sudo nano tenfile: CTRL+o - lưu nội dung file sau khi edit; CTRL+x - thoát edit file
- Xem ip của máy Ubuntu: ip -4addr
3. Cài đặt docker cho Ubuntu
4. Kiểm tra phiên bản docker vừa cài đặt, kiểm tra phiên bản docker compose
5. Cấu hình để docker chạy được mà không cần tiền tố sudo
6. Tìm hiểu tập lệnh của docker và docker compose
7. Đảm bảo tường lửa trên Ubuntu đã cho phép cổng 80, 1880, 9630 (Lệnh: sudo ufw allow ...)


## BÀI LÀM.
#### I. Cài đặt hệ điều hành Unbuntu 24.04.4 LTS
#### 1. Truy cập vào: https://ubuntu.com/download/desktop để download
- Chọn Download để tải file iso Ubuntu 24.04.4 LTS về máy
#### 2. Sử dụng VM_Ware để giả lập
##### Tạo máy ảo
##### Mở VM_Ware, vào file chọn Create New Virtual Machine

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/031e4fb9-0387-4576-93ad-d267e48b22dc" />

##### Chọn Installer disc image file (iso), sau đó chọn file iso đã tải trước đó

<img width="1917" height="1080" alt="image" src="https://github.com/user-attachments/assets/86d7ac75-3c6d-4e97-bfa0-b9dbe1a0424e" />

#### 3. Cấu hình mạng trong Ubuntu cho phép SSH truy cập được từ CMD của windows

#### Gõ lệnh sau trên cửa sổ ubuntu trên máy ảo VM_Ware để lấy ip máy
```ip a```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ed3ca83a-c69d-4c77-9435-fef7a02442e9" />

- IP là 192.168.126.131
- Sau khi lấy được IP, mở cmd và gõ lệnh dưới đây để truy cập SSH vào Ubuntu
- ssh nguyet@192.168.126.132
- Tiếp đến nhập password, nếu đăng nhập thành công thì sẽ chuyển cửa sổ vào terminal của Ubuntu
  
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/148dcc80-b099-4cd3-8f2e-5c1d01d4505a" />

#### II. Danh sách các lệnh Ubuntu cơ bản

| Lệnh | Chức năng | Cú pháp / Ví dụ |
| :--- | :--- | :--- |
| **ls** | Liệt kê file và thư mục | `ls` hoặc `ls -la` (xem file ẩn) |
| **mkdir** | Tạo thư mục mới | `mkdir nameFolder` |
| **cd** | Chuyển thư mục làm việc | `cd path` (VD: `cd /var/www`) |
| **cp** | Copy file | `cp file_nguon path/file_dich` |
| **sudo chmod** | Thay đổi quyền file | `sudo chmod xxx filename` (VD: `755`) |
| **sudo nano** | Chỉnh sửa file | `sudo nano tenfile` |
| **ip -4 addr** | Xem địa chỉ IP (IPv4) | `ip -4 addr` |

##### Phím tắt trong Nano Editor:
* **CTRL + O**: Lưu nội dung (Write Out).
* **CTRL + X**: Thoát khỏi trình chỉnh sửa.
* **Enter**: Xác nhận tên file khi lưu.
#### III. Cài đặt docker cho Ubuntu
#### Sử dụng lệnh: sudo apt update để cập nhật danh sách phần mềm

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b28870ad-4cdb-4e6d-91f7-d0ae341b4761" />

#### Cài đặt Docker và Docker compose
- Sử dụng lệnh sudo apt install docker.io docker-compose -y  để cài đặt docker và docker compose
- Sau khi cài đặt xong, sử dụng lệnh hai lệnh dưới đây để kiểm tra phiên bản docker và docker compose vừa cài đặt
```
docker --version
docker-compose --version
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1f4af749-597c-4e60-a348-560b69b13d29" />

### IV. Cấu hình để docker để chạy mà không cần tiền tố sudo
#### Bước 1. Thêm user vào group docker
- Sử dụng lệnh sudo usermod -aG docker $USER để thêm user vào group docker (thay $USER bằng tên người dùng của mình, ví dụ: nguyet)
- Sau khi đã thêm user vào group, ta cần đăng xuất ra và đăng nhập lại để quyền có hiệu lực. Hoặc có thể dùng lệnh newgrp docker để không cần thoát SSH ra để đăng nhập lại mà áp dụng quyền được luôn.

<img width="1920" height="1023" alt="image" src="https://github.com/user-attachments/assets/affc1000-245c-4592-828f-a2419a592f0a" />

=> Kết quả sau khi cấu hình để docker chạy mà không cần tiền tố sudo

### V. Danh sách các lệnh Docker & Docker Compose cơ bản

#### 1. Docker (Quản lý Container và Image)

| Lệnh | Chức năng | Ví dụ |
| :--- | :--- | :--- |
| **docker ps** | Liệt kê các container đang chạy | `docker ps` (Thêm `-a` để xem tất cả) |
| **docker images** | Liệt kê các image hiện có | `docker images` |
| **docker run** | Tạo và chạy một container từ image | `docker run -d --name my-app nginx` |
| **docker stop** | Dừng một container đang chạy | `docker stop <container_id>` |
| **docker rm** | Xóa một container | `docker rm <container_id>` |
| **docker rmi** | Xóa một image | `docker rmi <image_id>` |
| **docker exec** | Chạy lệnh bên trong container | `docker exec -it <id> bash` |
| **docker logs** | Xem log của container | `docker logs -f <id>` |

---

#### 2. Docker Compose (Quản lý đa Container)

| Lệnh | Chức năng | Lưu ý |
| :--- | :--- | :--- |
| **docker-compose up** | Khởi tạo và chạy các dịch vụ | Thêm `-d` để chạy ngầm (detached mode) |
| **docker-compose down** | Dừng và xóa các container, network | Dùng khi muốn dọn dẹp môi trường |
| **docker-compose ps** | Liệt kê trạng thái các dịch vụ | Phải chạy trong thư mục có file `.yml` |
| **docker-compose logs** | Xem log của tất cả dịch vụ | `docker-compose logs -f` để theo dõi thực tế |
| **docker-compose build** | Build hoặc build lại các dịch vụ | Dùng khi bạn thay đổi Dockerfile |
| **docker-compose restart** | Khởi động lại các dịch vụ | `docker-compose restart <service_name>` |

---

### Mẹo nhỏ cho Docker:
* **Dọn dẹp hệ thống:** `docker system prune` (Xóa tất cả container đã dừng, network và image thừa).
* **Phân biệt:** `docker` dùng cho từng container lẻ, `docker-compose` dùng khi bạn có file cấu hình `docker-compose.yml` để quản lý nhiều dịch vụ (như Web + DB) cùng lúc.
#### VI. Cấu hình tường lửa trên Ubuntu cho phép các cổng 80, 1880, 9630 (Lệnh: sudo ufw allow ...)
- Chạy các lệnh sau trong cửa sổ powrshell của ubuntu để cho phép các cổng 80, 1880, 9630 hoạt động
  
```
sudo ufw allow 80/tcp
sudo ufw allow 1880/tcp
sudo ufw allow 9630/tcp
```

<img width="1920" height="1076" alt="image" src="https://github.com/user-attachments/assets/44420589-8833-474f-b88a-929a552115ff" />

- Sau khi chạy xong 3 lệnh trên, chạy tiếp lệnh dưới đây để kích hoạt tường lửa:
```
sudo ufw enable
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6b0e4722-000f-489c-ad6e-496a98e01ce0" />



