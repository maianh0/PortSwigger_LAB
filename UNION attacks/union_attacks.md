<img width="1864" height="955" alt="image" src="https://github.com/user-attachments/assets/de26107c-2fb1-416c-8e43-ed6d91f79e39" /># Lab: SQL injection UNION attack, finding a column containing text

 - Để thực hiện tấn công UNION SELECT, trước tiên bạn cần xác định:
  + Số lượng cột được trả về bởi truy vấn gốc.
  + Cột nào có kiểu dữ liệu là chuỗi (string) – vì giá trị cần hiển thị trong lab là dạng chuỗi.
**Bước 1: Chặn request lọc danh mục bằng Burp Suite**
- Dùng Burp Suite để intercept (chặn) request khi chọn danh mục sản phẩm. Tìm tham số category
  ```
  GET /filter?category=Gifts
  ```
<img width="1919" height="877" alt="image" src="https://github.com/user-attachments/assets/41962070-b567-4765-a575-f9806aa05f7a" />

**Bước 2: Xác định số lượng cột trong truy vấn**
Sử dụng payload thử với nhiều giá trị NULL (câu lệnh láy ở bài lab trước(Lab: SQL injection UNION attack, determining the number of columns returned by the query)):
```
 GET /filter?category=Gifts'+UNION+SELECT+NULL,NULL,NULL--
```
**Bước 3: Tìm cột có kiểu dữ liệu chuỗi**
- Lần lượt thay từng NULL bằng 'abcdef'
<img width="1919" height="652" alt="image" src="https://github.com/user-attachments/assets/9601a288-b71a-4fce-9e0f-bae59214865f" />
- Nếu không hiển thị, thử cột 2
<img width="1919" height="872" alt="image" src="https://github.com/user-attachments/assets/5d7e6df1-070b-40b3-8bbc-6d374a1bb659" />
> Nếu payload nào hiển thị giá trị abcdef trên trang, thì đã tìm đúng cột có kiểu dữ liệu string, sau đó thay giá trị chuôi cho trên màn hình vào vị trí 'abcdef'
<img width="1874" height="986" alt="image" src="https://github.com/user-attachments/assets/9ddc271c-f856-43c7-b41c-1faf7ec58346" />


# Lab: SQL injection UNION attack, retrieving data from other tables
- Khai thác lỗi SQL Injection ở tham số lọc danh mục sản phẩm (category).
- Dùng kỹ thuật UNION SELECT để truy xuất dữ liệu từ bảng users.
- Tìm tài khoản admin và đăng nhập vào hệ thống bằng tài khoản đó.

**Bước 1: Chặn request và xác định tham số đầu vào**
- Mở Burp Suite → bật Proxy Intercept, truy cập website của lab và click vào danh mục Accessories và chuyển sang Repeater 
- Lân lượt thử lệnh để kiểm tra số cột của bảng
  ```
  filter?category=Accessories'+order+by+1--
  ```
  <img width="1918" height="986" alt="image" src="https://github.com/user-attachments/assets/03b0eefd-7d96-4530-8873-f3723c2ebdc4" />
  - Lần lượt thử cho đến khi nào trang web bị lỗi (500 hoặc trắng trang), thì đã vượt quá số cột → số cột hợp lệ là số cuối trước đó.
  <img width="1915" height="984" alt="image" src="https://github.com/user-attachments/assets/0405482b-6a87-4136-871e-d35413353fbd" />
  <img width="1913" height="679" alt="image" src="https://github.com/user-attachments/assets/c6ef7754-5a99-4dd1-8605-9ae96bc62e0c" />
> Số 3 đã vượt quá số cột nên ta tìm được số cột là 2

**Bước 2: Tìm cột nào chứa dữ liệu dạng văn bản (text)**
```
'+UNION+SELECT+'abc',NULL--
'+UNION+SELECT+'abc','dfg'--
```
<img width="1723" height="909" alt="image" src="https://github.com/user-attachments/assets/c0192afa-3416-4469-8c75-7e8d81852cf1" />
<img width="1864" height="955" alt="image" src="https://github.com/user-attachments/assets/6424e51e-d4e4-4818-b000-3a94f3df4c46" />
>Thấy 'abc' và 'def' hiện ra trong danh sách sản phẩm => Cả 2 đều hiển thị text

**Bước 3: Lấy dữ liệu từ bảng users**
```
' UNION SELECT username, password FROM users--
```
<img width="1919" height="929" alt="image" src="https://github.com/user-attachments/assets/f59b83bd-7dbb-44e6-9107-9dd6c55bd029" />
<img width="1858" height="982" alt="image" src="https://github.com/user-attachments/assets/9e3b4a48-7d09-4000-9b45-f568b2d8489a" />
>  Trang web sẽ hiển thị tài khoản và mật khẩu.

**Bước 4: Đăng nhập với tài khoản administrator**
- Tài khoản: administrator
- Password: xi26qgvs02yq3aoj2etq

<img width="1859" height="921" alt="image" src="https://github.com/user-attachments/assets/ac6dc309-a497-40e1-8c28-881d3bd08db0" />
> Đăng nhập thành công
