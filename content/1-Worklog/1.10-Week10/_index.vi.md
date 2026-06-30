---
title: "Nhật ký tuần 10"
date: 2026-06-22
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

{{% notice info %}}
Báo cáo này tóm tắt quá trình hoàn thiện chức năng QR Check-in và xây dựng module Certificate cho hệ thống AWS Event Management Platform.
{{% /notice %}}

### Mục tiêu tuần 10

- Hoàn thiện quy trình Check-in bằng mã QR.
- Xây dựng chức năng tạo Certificate dưới dạng PDF.
- Lưu trữ Certificate trên Amazon S3.
- Hoàn thiện quá trình tích hợp giữa Frontend và Backend.

---

### Công việc theo từng ngày

| Ngày | Nội dung |
|------|----------|
| Thứ 2 | Hoàn thiện quy trình QR Check-in, cập nhật trạng thái điểm danh trên Amazon DynamoDB và kiểm tra lại toàn bộ luồng điểm danh. |
| Thứ 3 | Tích hợp Amazon API Gateway với AWS Lambda và kiểm thử quá trình giao tiếp giữa Frontend và Backend. |
| Thứ 4 | Thiết kế mẫu Certificate và xây dựng chức năng tạo Certificate PDF sau khi người tham gia hoàn thành sự kiện. |
| Thứ 5 | Lưu trữ Certificate trên Amazon S3 và kiểm tra khả năng truy cập phục vụ tải xuống trong các bước tiếp theo. |
| Thứ 6 | Tích hợp quy trình tạo Certificate vào hệ thống và chuẩn bị cho chức năng gửi Email trong giai đoạn tiếp theo. |
| Thứ 7 | Kiểm thử toàn bộ chức năng Check-in và Certificate, sửa các lỗi phát sinh và tối ưu thời gian xử lý của AWS Lambda. |
| Chủ nhật | Cập nhật tài liệu API, rà soát toàn bộ luồng hoạt động của hệ thống và chuẩn bị các công việc cho tuần tiếp theo. |

---

### Thực hành

- Cập nhật trạng thái điểm danh trên Amazon DynamoDB.
- Tích hợp Amazon API Gateway với AWS Lambda.
- Xây dựng chức năng tạo Certificate dưới dạng PDF.
- Lưu trữ Certificate trên Amazon S3.
- Kiểm thử quy trình QR Check-in.
- Khắc phục các lỗi tích hợp giữa React và AWS Lambda.
- Sử dụng AWS SAM CLI và Docker để kiểm thử trên môi trường cục bộ.

---

### Kiến thức đạt được

- Amazon S3.
- Tạo tài liệu PDF.
- Tích hợp AWS Lambda.
- Amazon API Gateway.
- Amazon DynamoDB.
- AWS SAM CLI.
- Docker.
- Tích hợp ứng dụng Serverless.

---

### Kết quả đạt được

- Hoàn thiện quy trình Check-in bằng mã QR.
- Xây dựng thành công chức năng tạo Certificate dưới dạng PDF.
- Lưu trữ Certificate trên Amazon S3.
- Cải thiện quá trình tích hợp giữa Frontend và Backend.
- Hoàn thành kiểm thử và khắc phục các lỗi chính trước khi tiếp tục phát triển các chức năng còn lại của hệ thống.