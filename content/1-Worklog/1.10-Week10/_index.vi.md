---
title: "Worklog Tuần 10"
date: 2026-06-22
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

{{% notice info %}}
Báo cáo này tổng hợp quá trình hoàn thiện module Attendance & Certificate của dự án AWS Event Management Platform trong tuần thứ mười.
{{% /notice %}}

## Mục tiêu tuần 10

- Hoàn thiện chức năng Check-in bằng QR Code.
- Xây dựng chức năng tạo Certificate PDF sau khi tham gia sự kiện.
- Lưu trữ chứng nhận trên Amazon S3.
- Kiểm thử và tối ưu quy trình Attendance.

---

## Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|---|---|---|---|---|
| 2 | - Hoàn thiện Lambda xử lý Check-in bằng QR Code.<br>- Cập nhật trạng thái điểm danh trên Amazon DynamoDB.<br>- Kiểm thử luồng Check-in với nhiều trường hợp dữ liệu. | 22/06/2026 | 22/06/2026 | Dự án AWS Event Management |
| 3 | - Tích hợp Amazon API Gateway với Lambda.<br>- Hoàn thiện luồng giao tiếp giữa Frontend và Backend.<br>- Kiểm tra dữ liệu trả về sau khi Check-in. | 23/06/2026 | 23/06/2026 | Dự án AWS Event Management |
| 4 | - Thiết kế và xây dựng chức năng tạo Certificate PDF.<br>- Tạo mẫu chứng nhận sau khi người tham gia hoàn thành sự kiện.<br>- Kiểm thử quá trình sinh PDF. | 24/06/2026 | 24/06/2026 | Dự án AWS Event Management |
| 5 | - Lưu trữ Certificate trên Amazon S3.<br>- Chuẩn bị dữ liệu phục vụ tải xuống hoặc gửi qua email.<br>- Kiểm tra quyền truy cập và khả năng tải tệp. | 25/06/2026 | 26/06/2026 | Dự án AWS Event Management |
| 6 | - Kiểm thử toàn bộ module Attendance & Certificate.<br>- Khắc phục các lỗi phát sinh trong quá trình tích hợp.<br>- Cập nhật tài liệu API và tài liệu thiết kế của module. | 27/06/2026 | 28/06/2026 | Dự án AWS Event Management |

---

## Kết quả đạt được tuần 10

- Hoàn thiện chức năng Check-in bằng QR Code và cập nhật trạng thái điểm danh trên Amazon DynamoDB.
- Tích hợp thành công Amazon API Gateway với AWS Lambda để xử lý quy trình điểm danh.
- Xây dựng chức năng tạo Certificate PDF sau khi người tham gia hoàn thành sự kiện.
- Lưu trữ chứng nhận trên Amazon S3 và chuẩn bị dữ liệu cho việc tải xuống hoặc gửi qua email.
- Kiểm thử và tối ưu quy trình Check-in nhằm đảm bảo tính ổn định khi tích hợp với Frontend.
- Hoàn thiện tài liệu API và cập nhật tài liệu thiết kế cho module Attendance & Certificate.