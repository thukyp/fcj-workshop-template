---
title: "Nhật ký tuần 9"
date: 2026-06-15
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

{{% notice info %}}
Báo cáo này tóm tắt quá trình phát triển module Attendance của hệ thống AWS Event Management Platform theo kiến trúc Serverless.
{{% /notice %}}

### Mục tiêu tuần 9

- Thiết kế module quản lý điểm danh.
- Xây dựng quy trình Check-in bằng mã QR.
- Phát triển Backend bằng AWS Lambda.
- Tích hợp Frontend với các dịch vụ Serverless.

---

### Công việc theo từng ngày

| Ngày | Nội dung |
|------|----------|
| Thứ 2 | Phân tích yêu cầu của module Attendance, thống nhất cấu trúc API với các thành viên trong nhóm và hoàn thiện quy trình điểm danh. |
| Thứ 3 | Thiết kế Attendance Table trên Amazon DynamoDB, xác định Partition Key, các thuộc tính và cấu trúc lưu trữ dữ liệu điểm danh. |
| Thứ 4 | Phát triển hàm Check-in bằng AWS Lambda sử dụng AWS SAM, xây dựng logic kiểm tra và xác thực mã QR. |
| Thứ 5 | Cấu hình Amazon API Gateway và tích hợp với AWS Lambda để cung cấp API cho Frontend. |
| Thứ 6 | Xây dựng giao diện QR Check-in dành cho quản trị viên bằng React và tích hợp với các API đã triển khai. |
| Thứ 7 | Kiểm thử toàn bộ luồng React → API Gateway → Lambda → DynamoDB, đồng thời sửa các lỗi phát sinh trong quá trình kiểm thử. |
| Chủ nhật | Rà soát quy trình điểm danh, tối ưu xử lý trong Lambda và cập nhật tài liệu thiết kế của dự án. |

---

### Thực hành

- Thiết kế Attendance Table trên Amazon DynamoDB.
- Phát triển AWS Lambda bằng AWS SAM CLI.
- Cấu hình Amazon API Gateway.
- Tích hợp React với các dịch vụ Serverless.
- Sử dụng Docker để kiểm thử Lambda trên môi trường cục bộ.
- Lưu dữ liệu điểm danh trên Amazon DynamoDB.
- Kiểm thử toàn bộ quy trình Check-in bằng QR Code.

---

### Kiến thức đạt được

- AWS Lambda.
- Amazon API Gateway.
- Amazon DynamoDB.
- AWS SAM CLI.
- Docker.
- Tích hợp REST API.
- Quy trình xác thực QR Code.

---

### Kết quả đạt được

- Hoàn thành phiên bản đầu tiên của module Attendance.
- Kết nối thành công React với API Gateway và AWS Lambda.
- Xây dựng được quy trình Check-in bằng mã QR hoạt động ổn định.
- Lưu trữ dữ liệu điểm danh trên Amazon DynamoDB.
- Tạo nền tảng để triển khai chức năng Certificate trong tuần tiếp theo.