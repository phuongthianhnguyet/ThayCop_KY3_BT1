# A. Đăng kí tên miền xịn cho cá nhân
1. Đăng ký Domain xịn
2. Đăng ký tài khoản cloudflare
3. Thêm Domain đã đăng ký vào trong cloudflare: Nhập 2 dòng namespace
4. Nhập 2 dòng namespace của cloudfare vào trong trang quản lý DNS record của tên miền đăng ký (vd trên mắt bão)
## BÀI LÀM

### 1. Đăng ký Domain xịn.

#### Truy cập vào website của nhà cung cấp Domain (Ví dụ: mắt bão, iNET,...)

- Link website đăng ký: https://www.matbao.net/ten-mien/ten-mien-mien-phi.html
  
#### Nhập tên miền để kiểm tra xem đã tồn tại hay chưa, nếu đã tồn tại rồi thì phải đổi tên miền khác

- Xác thực tài khoản
  
- Sau khi xác thực thành công, tên miền sẽ được đưa vào hàng đợi đăng ký, kiểm tra đủ điều kiện sẽ được kích hoạt
  
### 2. Đăng ký tài khoản Cloudfare.

#### Truy cập vào cloudfare -> Nhấn Sign up -> Nhập email và password -> Tick Verify -> Nhấn create Account

### 3. Thêm Domain đã đăng ký vào cloudfare.

#### Sau khi đã tạo xong tài khoản, đăng nhập vào và chọn Domains -> add a site -> Nhập domain đã đăng ký vào -> continue


<img width="959" height="540" alt="1" src="https://github.com/user-attachments/assets/df5234ea-97ad-4565-a46a-804ea955694c" />

#### Chọn Free plan

<img width="960" height="540" alt="2" src="https://github.com/user-attachments/assets/58936207-e327-4c97-962d-de5b48dd8428" />

#### Nhấn continue to activation.

#### Sau khi nhấn continue to activation sẽ chuyển sang trang có chứa 2 dòng namespace, copy 2 dòng đó lại.

### 4. Nhập 2 dòng namespace của cloudflare vào trong trang quản lý DNS record của tên miền đăng ký.

#### Đăng nhập vào trang mắt bão https://manage.matbao.net/domains

#### Sau khi đăng nhập vào, chọn tên miền -> quản lý tên miền.

#### Chọn quản lý chi tiết dịch vụ

#### Trong Servername chọn "Sử dụng Name Server tùy chỉnh" sau đó nhập 2 namespace của cloudfare vào -> Nhấn lưu thay đổi
<img width="1915" height="950" alt="image" src="https://github.com/user-attachments/assets/05558b82-e7b7-43d1-a68f-12a08a9970db" />

#### Sau khi nhập 2 dòng namespace của cloudflare vào trong trang quản lý DNS record của tên miền đăng ký trên mắt bão, quay trở lại trang cloudfare kiểm tra, nếu status active => Đã kết nối thành công Domain với Cloudfare.

<img width="1920" height="955" alt="image" src="https://github.com/user-attachments/assets/01ab0df6-5212-4ca2-b760-fb2235f9aeb4" />
