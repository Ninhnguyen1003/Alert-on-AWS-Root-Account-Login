## Bước 1: Bật CloudTrail & Gửi Log vào CloudWatch

- **Mục tiêu:** Ghi lại toàn bộ hoạt động trong tài khoản và truyền trực tiếp vào CloudWatch để phân tích theo thời gian thực.
- **Đường dẫn cấu hình:**
  ```
  CloudTrail ➔ Trails ➔ Create Trail
  ```
- **Cài đặt quan trọng:** Bật tùy chọn `Log file delivery to CloudWatch Logs group`.

---

## Bước 2: Tạo CloudWatch Metric Filter

- **Mục tiêu:** Định nghĩa một pattern cụ thể để lọc riêng các sự kiện đăng nhập Root khỏi các log hệ thống thông thường.
- **Filter Pattern:**
  ```json
  { $.userIdentity.type = "Root" && $.eventType != "AwsServiceEvent" }
  ```
- **Cấu hình Metric:**
  - Tên Metric: `RootAccountLoginCount`
  - Namespace: `Security`
  - Giá trị: `1` *(Tăng thêm 1 mỗi khi có kết quả khớp)*

---

## Bước 3: Tạo CloudWatch Alarm

- **Mục tiêu:** Thiết lập ngưỡng kích hoạt cảnh báo ngay khi phát hiện sự kiện.
- **Logic:** Kích hoạt cảnh báo nếu `RootAccountLoginCount >= 1` trong bất kỳ khoảng thời gian 5 phút nào.
- **Ý nghĩa:** BẤT KỲ lần đăng nhập root nào cũng sẽ ngay lập tức chuyển trạng thái cảnh báo sang `ALARM`.

---

## Bước 4: Cấu Hình Thông Báo qua SNS & Tự Động Hóa

- **Mục tiêu:** Đảm bảo đội bảo mật được thông báo ngay lập tức và tùy chọn thực thi các biện pháp xử lý tự động.
- **Các hành động:**
  - **SNS:** Gửi cảnh báo ngay đến Đội Bảo Mật qua Email + SMS.
  - **Tự động hóa nâng cao (Tùy chọn):** Kích hoạt hàm AWS Lambda để tự động vô hiệu hóa/cách ly thông tin xác thực root hoặc khởi động quy trình phản hồi sự cố.

---

![Thiết lập CloudWatch Alarm](Screenshot%202026-06-15%20191137.png)```
