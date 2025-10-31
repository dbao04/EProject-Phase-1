# 🛒 Hệ thống Thương mại Điện tử - Kiến trúc Microservices

Dự án mô phỏng một **nền tảng thương mại điện tử cơ bản** được xây dựng theo **kiến trúc Microservices**. Hệ thống bao gồm nhiều dịch vụ tách biệt, mỗi dịch vụ đảm nhiệm một nghiệp vụ riêng biệt như xác thực người dùng, quản lý sản phẩm và xử lý đơn hàng. Toàn bộ hệ thống được triển khai bằng **Docker** để đảm bảo khả năng mở rộng và dễ dàng quản lý.

---

## 🧩 Tổng quan Kiến trúc

Dự án gồm 4 dịch vụ chính hoạt động độc lập:

* **`api-gateway`**: Là điểm truy cập duy nhất của toàn hệ thống, tiếp nhận mọi request từ client và định tuyến đến các dịch vụ tương ứng.
* **`auth-service`**: Phụ trách việc xác thực người dùng — bao gồm đăng ký, đăng nhập và phát hành JSON Web Token (JWT).
* **`product-service`**: Đảm nhiệm việc quản lý dữ liệu sản phẩm như tạo mới, chỉnh sửa hoặc liệt kê sản phẩm.
* **`order-service`**: Xử lý các quy trình liên quan đến việc tạo và quản lý đơn hàng.

Mỗi dịch vụ đều có cơ sở dữ liệu riêng (MongoDB) và chúng giao tiếp nội bộ thông qua mạng do Docker Compose tạo ra.

---

## ⚙️ Cài đặt & Khởi chạy

### **Yêu cầu**
- Cần cài đặt [Docker](https://www.docker.com/products/docker-desktop/) và Docker Compose.

### **Cách chạy dự án**
1. Clone repository về máy.
2. Mở Terminal hoặc PowerShell tại thư mục gốc của dự án.
3. Chạy lệnh:
   ```bash
   docker-compose up --build
   ```
4. Khi hệ thống hoạt động thành công, bạn sẽ thấy log thông báo các service đã kết nối tới MongoDB và sẵn sàng lắng nghe trên các cổng tương ứng.

---

## 🧪 Kiểm thử API bằng Postman

Sau khi khởi động hệ thống, bạn có thể kiểm tra các API thông qua Postman.  
Tất cả request đều gửi tới **API Gateway (cổng 8088)**.

### **1. Đăng ký tài khoản**
* **Method**: `POST`  
* **URL**: `http://localhost:8088/auth/register`  
* **Body**: `raw (JSON)`  
  ```json
  {
      "username": "testuser",
      "password": "password123"
  }
  ```

### **2. Đăng nhập**
* **Method**: `POST`  
* **URL**: `http://localhost:8088/auth/login`  
* **Body**:  
  ```json
  {
      "username": "testuser",
      "password": "password123"
  }
  ```
* **Kết quả**: Sao chép giá trị `accessToken` trong phản hồi.

### **3. Tạo sản phẩm**
* **Method**: `POST`  
* **URL**: `http://localhost:8088/products`  
* **Authorization**: Chọn `Bearer Token` và dán `accessToken` ở trên.  
* **Body**:  
  ```json
  {
      "name": "Macbook Pro M4",
      "price": 50000000,
      "description": "Chip siêu cấp tốc độ cao"
  }
  ```
* **Lưu ý**: Ghi lại `_id` của sản phẩm mới.

### **4. Tạo đơn hàng**
* **Method**: `POST`  
* **URL**: `http://localhost:8088/orders`  
* **Authorization**: Dùng `Bearer Token`.  
* **Body**:  
  ```json
  {
      "products": ["<ID_SẢN_PHẨM_VỪA_TẠO>"],
      "totalPrice": 50000000
  }
  ```

---

## 💡 Công nghệ được sử dụng

* **Ngôn ngữ & Framework**: Node.js, Express.js  
* **Cơ sở dữ liệu**: MongoDB (qua Mongoose)  
* **Kiến trúc**: Microservices, API Gateway  
* **Triển khai**: Docker, Docker Compose  
* **Xác thực**: JSON Web Token (JWT)
