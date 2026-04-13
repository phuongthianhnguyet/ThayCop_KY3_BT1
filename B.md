# Cài đặt Ubuntu + Docker
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

<img width="1920" height="1080" alt="image" src="https://github.com/user attachments/assets/fcca9bdd-5d9e-461f-a16d-0262767db6a6" />
#### 3. Cấu hình mạng trong Ubuntu cho phép SSH truy cập được từ CMD của windows

#### Gõ lệnh sau trên cửa sổ ubuntu trên máy ảo VM_Ware để lấy ip máy
```ip a```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ed3ca83a-c69d-4c77-9435-fef7a02442e9" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/148dcc80-b099-4cd3-8f2e-5c1d01d4505a" />

#### II.
#### III. 
#### Sử dụng lệnh: sudo apt update để cập nhật danh sách phần mềm
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b28870ad-4cdb-4e6d-91f7-d0ae341b4761" />
#### Cài đặt Docker và Docker compose
- Sử dụng lệnh sudo apt install docker.io docker-compose -y  để cài đặt docker và docker compose
- Sau khi cài đặt xong, sử dụng lệnh hai lệnh dưới đây để kiểm tra phiên bản docker và docker compose vừa cài đặt
```docker --version
docker-compose --version ```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/43b8449c-77a4-413a-8b61-c4836610226f" />

#### IV. Cấu hình để docker để chạy mà không cần tiền tố sudo



