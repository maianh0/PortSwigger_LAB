# Lab: SQL injection UNION attack, finding a column containing text

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



