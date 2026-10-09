# Báo Cáo Thực Hành: Kiểm Thử API Cơ Bản Với Postman

![Postman](https://img.shields.io/badge/Postman-FF6C37?style=for-the-badge&logo=postman&logoColor=white)
![JSON](https://img.shields.io/badge/JSON-000000?style=for-the-badge&logo=json&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

** Thông tin sinh viên:**
* **Họ và tên:** Trần Tiến Đức
* **Mã sinh viên:** 23010777

** Mô tả dự án:**
Dự án này lưu trữ báo cáo và kết quả thực hành các thao tác kiểm thử API cơ bản bằng Postman. Bài tập sử dụng `JSONPlaceholder API` làm môi trường thử nghiệm, bao gồm các thao tác: thiết lập workspace, gửi HTTP request, truyền tham số và viết kịch bản test tự động bằng JavaScript.

---

##  Mục Lục
1. [Thiết lập Workspace & Collection](#1-thiết-lập-workspace--collection)
2. [Gửi API Request (GET)](#2-gửi-api-request-get)
3. [Truyền Tham Số (Query Params)](#3-truyền-tham-số-query-params)
4. [Kịch Bản Tự Động Kiểm Thử](#4-kịch-bản-tự-động-kiểm-thử-test-automation)
5. [Chạy Collection Runner](#5-chạy-collection-runner)
6. [Tổng Kết](#6-tổng-kết)

---

## 1. Thiết lập Workspace & Collection
- **Thực hiện:** Tải, cài đặt ứng dụng Postman và đăng nhập thành công vào hệ thống.
- **Tạo Collection:** Đã khởi tạo một Collection mới với tên gọi `Test collection of apis` nhằm tổ chức và lưu trữ các request kiểm thử một cách có hệ thống.

<img width="1591" height="996" alt="download" src="https://github.com/user-attachments/assets/57cc1d54-0db2-4e59-9017-f88537ce9c0e" />


---

## 2. Gửi API Request (GET)
- **Mục tiêu:** Gửi một HTTP GET Request đến một Public API để lấy danh sách người dùng.
- **Endpoint:** `https://jsonplaceholder.typicode.com/users` (Sử dụng API dự phòng để khắc phục lỗi phân giải DNS `EAI_AGAIN`).
- **Kết quả:** Nhận về mảng dữ liệu định dạng JSON.
- **Kiểm tra trạng thái:** Xác nhận Status Code trả về là `200 OK` và Response Time nằm trong mức hợp lệ.

<img width="1584" height="1000" alt="download" src="https://github.com/user-attachments/assets/a3384065-dea8-41d1-9873-91bcb8d0890b" />


---

## 3. Truyền Tham Số (Query Params)
- **Truyền tham số:** Sử dụng Query Parameters để giới hạn số lượng kết quả trả về. Thiết lập Key `_limit` với Value là `3`.
- **Kết quả:** URL được tự động nối thêm tham số `?_limit=3`. Phần Body của Response phản hồi chính xác, chỉ chứa mảng rút gọn gồm 3 người dùng đầu tiên.
- **Khảo sát bổ sung:**
  - Kiểm tra cơ chế tự động quản lý (ẩn/hiện) các Headers mặc định của Postman.
  - Tìm hiểu cách cấu hình gửi file (Form-data) và gửi dữ liệu dạng Raw/JSON thông qua các phương thức `POST/PUT/PATCH`.

<img width="1596" height="998" alt="download" src="https://github.com/user-attachments/assets/9d2352c9-d9cd-45c4-8a8c-393cb93320c1" />


---

## 4. Kịch Bản Tự Động Kiểm Thử (Test Automation)
- **Thực hiện:** Tích hợp thẻ `Tests` kết hợp với thư viện Snippets (JavaScript) của Postman để viết kịch bản kiểm tra tự động.
- **Kịch bản (Scripts):** Kiểm tra mã trạng thái trả về (200) và xác thực kiểu dữ liệu của Body phải là một mảng (Array).
  ```javascript
  // Kiểm tra trạng thái 200 OK
  pm.test("Status code is 200", function () {
      pm.response.to.have.status(200);
  });

  // Xác thực dữ liệu trả về là Array có chứa phần tử
  pm.test("Dữ liệu trả về là danh sách người dùng", function () {
      var jsonData = pm.response.json();
      pm.expect(jsonData).to.be.an('array');
      pm.expect(jsonData.length).to.be.above(0);
  });
  ```
  <img width="1592" height="992" alt="download" src="https://github.com/user-attachments/assets/902a3cda-6ad9-4c9a-b34d-de9058dfb87d" />


  
  ```
- **Thử nghiệm lỗi (Fail test):** Chủ động thay đổi kỳ vọng thành `status(123)` để kiểm chứng cơ chế báo lỗi của Postman khi kết quả thực tế sai lệch với kịch bản.

<img width="1600" height="998" alt="download" src="https://github.com/user-attachments/assets/19e6f5be-77e9-4666-99c6-18a56c100d2e" />


---

## 5. Chạy Collection Runner
- **Thực hiện:** Sử dụng công cụ **Collection Runner** để thực thi tự động hàng loạt các request và kịch bản test có trong thư mục.
- **Cấu hình:** Chạy môi trường Local với 1 vòng lặp (Iteration). (Đã khắc phục lỗi "No tests found" bằng cách lưu (Save) request trước khi chạy).
- **Kết quả:** Hệ thống chạy thành công. Bảng thống kê ghi nhận các test case (Status code 200 và Kiểm tra mảng dữ liệu) đều đạt trạng thái `Passed`.

<img width="1596" height="994" alt="download" src="https://github.com/user-attachments/assets/5ce7f839-af2e-4f3f-b854-050feaf7fa33" />


---

