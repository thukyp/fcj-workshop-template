---
title: "Tuần 9"
date: 2026-06-15
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

{{% notice info %}}
Báo cáo này tổng hợp quá trình phát triển module Attendance & Certificate của dự án AWS Event Management Platform trong tuần thứ chín.
{{% /notice %}}

## Mục tiêu tuần 9

- Xây dựng chức năng điểm danh bằng QR Code.
- Thiết kế cơ sở dữ liệu phục vụ Attendance.
- Kết nối Frontend với Backend thông qua API Gateway.
- Hoàn thiện phiên bản đầu tiên của quy trình Check-in.

---

## Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|---|---|---|---|---|
| 2 | - Phân tích yêu cầu module Attendance & Certificate.<br>- Thảo luận API và mô hình dữ liệu với các thành viên trong nhóm.<br>- Thiết kế AttendanceTable trên Amazon DynamoDB. | 15/06/2026 | 15/06/2026 | Dự án AWS Event Management |
| 3 | - Khởi tạo Check-in Lambda.<br>- Xây dựng luồng xử lý kiểm tra QR Code bằng Ticket ID.<br>- Kiểm thử với dữ liệu mẫu trên DynamoDB. | 16/06/2026 | 16/06/2026 | Dự án AWS Event Management |
| 4 | - Thiết kế giao diện QR Check-in dành cho quản trị viên.<br>- Tích hợp thư viện quét QR và xử lý kết quả trả về.<br>- Kiểm thử luồng Check-in cơ bản. | 17/06/2026 | 17/06/2026 | Dự án AWS Event Management |
| 5 | - Kết nối API Gateway với AWS Lambda.<br>- Kiểm thử API từ Frontend.<br>- Điều chỉnh cấu trúc dữ liệu trả về giữa Backend và Frontend. | 18/06/2026 | 19/06/2026 | Dự án AWS Event Management |
| 6 | - Kiểm thử toàn bộ quy trình điểm danh.<br>- Ghi nhận các lỗi phát sinh và cập nhật tài liệu thiết kế module.<br>- Hoàn thiện phiên bản đầu tiên của chức năng Check-in. | 20/06/2026 | 21/06/2026 | Dự án AWS Event Management |

---

## Kết quả đạt được tuần 9

- Hoàn thành thiết kế AttendanceTable trên Amazon DynamoDB phục vụ chức năng điểm danh.
- Xây dựng thành công Lambda xử lý Check-in bằng QR Code.
- Hoàn thiện luồng xác thực Ticket ID và cập nhật trạng thái điểm danh.
- Thiết kế giao diện QR Check-in dành cho quản trị viên và tích hợp chức năng quét mã.
- Kết nối Frontend với AWS Lambda thông qua Amazon API Gateway.
- Kiểm thử thành công quy trình Check-in với dữ liệu mẫu và ghi nhận các lỗi cần tiếp tục cải thiện.
- Cập nhật tài liệu thiết kế và chuẩn bị cho việc phát triển chức năng Certificate ở tuần tiếp theo.