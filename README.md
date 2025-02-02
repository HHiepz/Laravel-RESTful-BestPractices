# Quy Chuẩn HTTP & RESTful API trong Laravel

## Mục lục
1. [Mục đích](#1-mục-đích)
2. [Quy tắc sử dụng HTTP Status Codes](#2-quy-tắc-sử-dụng-http-status-codes)
   - [Mã 2xx: Thành công](#21-mã-2xx-thành-công)
   - [Mã 4xx: Lỗi phía Client](#22-mã-4xx-lỗi-phía-client)
   - [Mã 5xx: Lỗi phía Server](#23-mã-5xx-lỗi-phía-server)
3. [Quy chuẩn thiết kế RESTful API](#3-quy-chuẩn-thiết-kế-restful-api)
   - [Quy tắc đặt tên URI](#31-quy-tắc-đặt-tên-uri) ✨
   - [Cấu trúc URI theo cấp bậc](#32-cấu-trúc-uri-theo-cấp-bậc) ✨
   - [Quy tắc trả dữ liệu JSON](#33-quy-tắc-trả-dữ-liệu-json)
   - [Quy tắc về versioning (Phiên bản hóa API)](#34-quy-tắc-về-versioning-phiên-bản-hóa-api)
4. [Quy chuẩn xử lý lỗi và trả về lỗi từ API](#4-quy-chuẩn-xử-lý-lỗi-và-trả-về-lỗi-từ-api)
   - [Cấu trúc phản hồi](#41-cấu-trúc-phản-hồi)
   - [Quy tắc tạo "message" và "errors"](#42-quy-tắc-tạo-message-và-errors)

---

## 1. Mục đích
Tài liệu này đặt ra quy chuẩn về việc sử dụng **HTTP status codes** và xây dựng **RESTful API** nhằm đảm bảo tính thống nhất, hiệu quả và dễ bảo trì cho dự án nhóm sử dụng **Laravel** làm backend. Tài liệu tuân thủ các tiêu chuẩn của **Microsoft REST API Guidelines** và các quy tắc phổ biến trong thiết kế API.

---

## 2. Quy tắc sử dụng HTTP Status Codes

### 2.1 Mã 2xx: Thành công
- **200 OK**: Sử dụng khi yêu cầu được xử lý thành công. Áp dụng cho các thao tác lấy, cập nhật, xóa hoặc thực hiện thành công trên server.
  - Ví dụ phản hồi:
    ```json
    {
      "status": "success",
      "message": "Resource retrieved successfully",
      "data": {
        "id": 1,
        "name": "Product Name",
        "price": 100
      }
    }
    ```
- **201 Created**: Sử dụng khi một resource mới được tạo thành công. Ví dụ: tạo tài khoản, tạo đơn hàng.
  - Ví dụ phản hồi:
    ```json
    {
      "status": "success",
      "message": "Resource created successfully",
      "data": {
        "id": 1,
        "name": "Product Name",
        "price": 100
      }
    }
    ```
- **204 No Content**: Khi yêu cầu xử lý thành công nhưng không có nội dung nào trả về, ví dụ: xóa tài nguyên.

[⬆ Quay lại Mục lục](#mục-lục)

### 2.2 Mã 4xx: Lỗi phía Client
- **400 Bad Request**: Sử dụng khi dữ liệu từ client gửi lên không hợp lệ (form không hợp lệ, JSON sai định dạng).
  - Ví dụ phản hồi:
    ```json
    {
      "status": "error",
      "message": "Invalid input data",
      "errors": {
        "email": ["The email field is required."],
        "password": ["The password must be at least 8 characters."]
      }
    }
    ```
- **401 Unauthorized**: Sử dụng khi người dùng chưa xác thực hoặc token không hợp lệ (Tức chưa đăng nhập).
  - Ví dụ phản hồi:
    ```json
    {
      "status": "error",
      "message": "Unauthorized",
      "errors": {
        "detail": "Authentication credentials were not provided."
      }
    }
    ```
- **403 Forbidden**: Sử dụng khi người dùng không có quyền truy cập tài nguyên. Dù đã đăng nhập (bị cấm truy cập).
  - Ví dụ phản hồi:
    ```json
    {
      "status": "error",
      "message": "Forbidden",
      "errors": {
        "detail": "You do not have permission to access this resource."
      }
    }
    ```
- **404 Not Found**: Khi không tìm thấy tài nguyên yêu cầu. Ví dụ: yêu cầu một sản phẩm không tồn tại.
  - Ví dụ phản hồi:
    ```json
    {
      "status": "error",
      "message": "Resource not found",
      "errors": {
        "detail": "The requested resource does not exist."
      }
    }
    ```
- **429 Too Many Requests**: Khi người dùng gửi quá nhiều yêu cầu trong một khoảng thời gian (rate limiting).
  - Ví dụ phản hồi:
    ```json
    {
      "status": "error",
      "message": "Too many requests",
      "errors": {
        "detail": "Please try again later."
      }
    }
    ```

[⬆ Quay lại Mục lục](#mục-lục)

### 2.3 Mã 5xx: Lỗi phía Server
- **500 Internal Server Error**: Lỗi không xác định từ server. Cần kiểm tra logs để xử lý (dùng `try` `catch` để bắt lỗi này).
  - Ví dụ phản hồi:
    ```json
    {
      "status": "error",
      "message": "Internal Server Error",
      "errors": {
        "detail": "An unexpected error occurred. Please try again later."
      }
    }
    ```
- **503 Service Unavailable**: Dùng khi server đang bảo trì hoặc quá tải, không thể xử lý yêu cầu.
  - Ví dụ phản hồi:
    ```json
    {
      "status": "error",
      "message": "Service Unavailable",
      "errors": {
        "detail": "The server is currently unavailable. Please try again later."
      }
    }
    ```

[⬆ Quay lại Mục lục](#mục-lục)

---

## 3. Quy chuẩn thiết kế RESTful API

### 3.1 Quy tắc đặt tên URI
URI (Uniform Resource Identifier) cần tuân thủ theo chuẩn **RESTful** và **Microsoft API Guidelines**, bao gồm:
- **Sử dụng danh từ thay vì động từ**: URI chỉ định tài nguyên chứ không phải hành động. Ví dụ:
  - **Đúng**: `/users`, `/products`
  - **Sai**: `/getUsers`, `/createProduct`
- **Dùng số nhiều cho tài nguyên**: Để rõ ràng và dễ hiểu, tài nguyên nên được đặt ở dạng số nhiều.
  - **Đúng**: `/products`, `/orders`
  - **Sai**: `/product`, `/order`
- **Sử dụng chữ thường và phân tách bằng dấu gạch ngang (-)**: Tránh sử dụng chữ hoa hoặc dấu gạch dưới.
  - **Đúng**: `/users`, `/product-details`
  - **Sai**: `/Users`, `/product_details`
- **Sử dụng các phương thức HTTP đúng chuẩn**:
  - `GET`: Lấy dữ liệu tài nguyên.
  - `POST`: Tạo mới tài nguyên.
  - `PUT`: Cập nhật toàn bộ tài nguyên.
  - `PATCH`: Cập nhật một phần tài nguyên.
  - `DELETE`: Xóa tài nguyên.

[⬆ Quay lại Mục lục](#mục-lục)

### 3.2 Cấu trúc URI theo cấp bậc
Cấu trúc URI cần rõ ràng theo cấp bậc, phản ánh mối quan hệ giữa các tài nguyên. Ví dụ:
- **GET** `/users/{user_id}/orders`: Lấy danh sách đơn hàng của một người dùng cụ thể.
- **POST** `/products/{product_id}/reviews`: Thêm mới một đánh giá cho một sản phẩm.
- **PUT** `/users/{user_id}/orders/{order_id}`: Cập nhật đơn hàng cho một người dùng cụ thể.
- **DELETE** `/users/{user_id}/orders/{order_id}`: Xóa đơn hàng của một người dùng.

[⬆ Quay lại Mục lục](#mục-lục)

### 3.3 Quy tắc trả dữ liệu JSON
- **Trả dữ liệu kèm HTTP Status**: Luôn luôn kèm trạng thái HTTP Status.
  - Ví dụ:
    - Lấy dữ liệu thành công
    ```php
    return response()->json([
        'status' => "success",
        'message' => 'Resource retrieved successfully',
        'data' => $resource
    ], 200);
    ```
    - Tạo thành công
    ```php
    return response()->json([
        'status' => "success",
        'message' => 'Resource created successfully',
        'data' => $resource
    ], 201);
    ```
    - Xóa thành công
    ```php
    return response()->noContent();  
    ```

- **Trả về đối tượng JSON**: Mọi dữ liệu trả về từ API phải là JSON, bao gồm thông tin về **status**, **message**, và **data** nếu có.
  - Ví dụ:
    ```json
    {
      "status": "success",
      "message": "Resource retrieved successfully",
      "data": {
        "id": 1,
        "name": "Product Name",
        "price": 100
      }
    }
    ```
- **Pagination (Phân trang)**: Khi trả về danh sách lớn dữ liệu, cần sử dụng phân trang và cung cấp thông tin như `current_page`, `total_pages`, `per_page`, `total_items`.
  - Ví dụ:
    ```json
    {
      "status": "success",
      "message": "Products retrieved successfully",
      "data": [
        {
          "id": 1,
          "name": "Product 1",
          "price": 100
        },
        {
          "id": 2,
          "name": "Product 2",
          "price": 150
        }
      ],
      "pagination": {
        "current_page": 1,
        "total_pages": 10,
        "per_page": 10,
        "total_items": 100
      }
    }
    ```

[⬆ Quay lại Mục lục](#mục-lục)

### 3.4 Quy tắc về versioning (Phiên bản hóa API)
- **Sử dụng versioning trong URI**: Để duy trì tính tương thích khi API được thay đổi, cần áp dụng version cho API. Version có thể được đặt ở đầu URI.
  - Ví dụ: `/v1/users`, `/v2/products`

[⬆ Quay lại Mục lục](#mục-lục)

---

## 4. Quy chuẩn xử lý lỗi và trả về lỗi từ API

### 4.1 Cấu trúc phản hồi
Phản hồi lỗi từ API phải theo chuẩn, cung cấp thông tin rõ ràng và chi tiết khi cần thiết, đặc biệt là trong form validation. Cấu trúc phản hồi bao gồm:
  - **status**: trạng thái (ví dụ: `"error"`).
  - **message**: mô tả ngắn gọn về lỗi.
  - **errors**: danh sách các lỗi chi tiết cho từng trường dữ liệu.
    - Ví dụ:
    ```json
    {
        "status": "error",
        "message": "Invalid input data",
        "errors": {
          "email": ["The email field is required."],
          "password": ["The password must be at least 8 characters."]
        }
    }
    ```

[⬆ Quay lại Mục lục](#mục-lục)

### 4.2 Quy tắc tạo "message" và "errors"
  - **Message**: Dùng một thông báo chung khi không cần chi tiết (VD: `"Resource not found"`).
  - **Errors**: Dùng cho chi tiết lỗi khi cần (VD: `"email": ["The email field is required."]`).

[⬆ Quay lại Mục lục](#mục-lục)