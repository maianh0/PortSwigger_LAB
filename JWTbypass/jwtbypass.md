# Lab: JWT authentication bypass via weak signing key
**Thực hiện**: Mai Anh  
**Cập nhật lần cuối**: 01/08/2025

## Mục lục
- [1. Mô tả](#1-mô-tả)
- [2. Mục tiêu](#2-mục-tiêu)
- [3. Các bước thực hiện](#3-các-bước-thực-hiện)


## 1. Mô tả

Phòng lab này sử dụng **JWT (JSON Web Token)** để xử lý phiên đăng nhập (session). Tuy nhiên, hệ thống lại sử dụng một **khóa bí mật (secret key) rất yếu** để thực hiện việc **ký và xác thực token**.

Do secret key quá đơn giản, kẻ tấn công có thể **brute-force** bằng cách sử dụng một **wordlist chứa các secret phổ biến**.

### JSON Web Token (JWT)
- JWT là chuẩn token dùng để truyền tải dữ liệu giữa client và server. Một JWT gồm 3 phần: <Header>.<Payload>.<Signature> 
- Trong đó:
  - Header: chứa thông tin về thuật toán ký (ví dụ: HS256).
  - Payload: chứa dữ liệu (claims) như sub, iss, exp.
  - Signature: chữ ký số xác thực tính toàn vẹn, được tạo từ Header + Payload và một secret key.

### Lỗ hổng: Weak Signing Key
Nếu server dùng một secret yếu (ví dụ: "secret", "admin", "123456"), attacker có thể brute-force được key → tạo JWT hợp lệ → giả mạo quyền truy cập.

## 2. Mục tiêu bài lab

- Phát hiện và khai thác lỗ hổng xác thực sử dụng JWT có khóa ký yếu.
- Thực hiện brute-force để tìm khóa bí mật (secret key).
- Sử dụng secret tìm được để ký lại JWT giả mạo với quyền `administrator`.
- Gửi token đã sửa để truy cập `/admin` và xóa người dùng `carlos`.

## 3. Các bước thực hiện
### Bước 1: Tìm secret key
- Truy cập phòng thí nghiệm và mở Burp Suite, tải extension JWT Editor và JSON Web Tokens từ cửa hàng BAPP.
- Đăng nhập vào tạo khoản bằng mật khẩu và tài khoản bài lab cung cấp "wiener:peter"
- Đăng nhập vào tài khoản, sau đó gửi request GET /my-account (sau đăng nhập) tới Burp Repeater.
  <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/15f4bfb9-b3ac-4f90-b636-049b21969185" />
- Trong Burp Repeater, đổi đường dẫn thành /admin và gửi request. Quan sát thấy rằng chỉ tài khoản administrator mới có thể truy cập trang admin panel.
  <img width="1919" height="714" alt="image" src="https://github.com/user-attachments/assets/cf08d666-5c63-47af-bda4-da9a6bbf5e78" />
- Copy JWT và tiến hành brute-force để tìm secret key
  <img width="928" height="242" alt="image" src="https://github.com/user-attachments/assets/274ff753-7bcc-4a7e-b256-20aa2de545a1" />
  > Tìm được secret key là **secret1**
### Bước 2: Tạo khóa kí giả mạo
**Cách 1** Sử dụng JSON Web Tokens
<img width="1919" height="846" alt="image" src="https://github.com/user-attachments/assets/73323d96-ff2e-49d0-82e0-ef1c87cfa278" />
- Chọn Recalculate Signature và nhaoaj secret key, sau đó sửa lại sub payload **wiener** thành **administrator** và nhấn Send
- Click chuột phải ben response và chọn Show response in browser và copy liên kết và dán vào thanh tìm kiếm
<img width="1919" height="599" alt="image" src="https://github.com/user-attachments/assets/bb109ed1-4621-460b-9d06-df185aacf05c" />
> Click delete user **carlos**  là hoàn thành bài lab

**Cách 2** Sử dụng python tạo khóa thủ công
- Copy JWT và dán vào trang jwt.io để có thông tin Header và Payload
  
<img width="1842" height="969" alt="image" src="https://github.com/user-attachments/assets/e2f7af83-24b1-4df0-afd7-03519630d4e1" />

- Sao chép thông tin Header và Payload vào đoạn code py tự tạo khóa kí giả mạo sau
  
  <img width="1687" height="903" alt="image" src="https://github.com/user-attachments/assets/53d49232-3f1c-494b-899f-3d76506d475f" />

- Chạy thử và thấy JWT in ra giống với JWT ban đầu là đúng, sau đó thay **wiener** thành **administrator** và chạy

<img width="1654" height="757" alt="image" src="https://github.com/user-attachments/assets/ad2dc73c-81a9-4c18-b711-572df67dd72a" />

- Copy mã JWT được in ra và paste vào session ở trang bài lab

<img width="1919" height="901" alt="image" src="https://github.com/user-attachments/assets/66659f62-3142-4f72-8461-a94767529514" />
<img width="1919" height="754" alt="image" src="https://github.com/user-attachments/assets/48778937-e966-45fd-8f95-33d051989d0a" />

- Sau khi dán session thì ta được như này
  <img width="1919" height="754" alt="image" src="https://github.com/user-attachments/assets/31ef5284-4a95-4fd1-bad8-999693c100ee" />
> Chọn delete user **carlos** là hoàn thành bài lab
<img width="1919" height="712" alt="image" src="https://github.com/user-attachments/assets/12ceb3ea-7a8c-45da-bd7e-c17f6a5da534" />


