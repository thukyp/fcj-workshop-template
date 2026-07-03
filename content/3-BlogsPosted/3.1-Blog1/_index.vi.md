---
title: "Blog 1"
date: 2026-06-20
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Xây dựng kiến trúc Notification và Analytics theo mô hình Serverless trên AWS

Trong quá trình phát triển **AWS Event Management Platform**, mình được giao phụ trách các module **Notification**, **Analytics** và **Deployment**. Ban đầu mình nghĩ đây chỉ là những chức năng hỗ trợ, tuy nhiên khi bắt đầu triển khai mới nhận ra chúng đóng vai trò rất quan trọng trong khả năng vận hành và mở rộng của toàn bộ hệ thống.

Thay vì triển khai theo mô hình máy chủ truyền thống, nhóm mình lựa chọn **kiến trúc Serverless** trên AWS nhằm giảm chi phí hạ tầng, hạn chế việc quản lý máy chủ và tận dụng khả năng tự động mở rộng của các dịch vụ AWS.

## Kiến trúc Event-Driven

Một trong những mục tiêu khi thiết kế hệ thống là giảm sự phụ thuộc giữa các module.

Thay vì để các chức năng gọi trực tiếp lẫn nhau, hệ thống xử lý theo mô hình **Event-Driven**. Khi một sự kiện như đăng ký thành công hoặc tạo vé hoàn tất xảy ra, hệ thống sẽ phát sinh một sự kiện để các thành phần liên quan tiếp tục xử lý.

Cách tiếp cận này giúp việc mở rộng chức năng trong tương lai trở nên đơn giản hơn mà không cần thay đổi các module hiện có.

---

## Notification Service

Module Notification chịu trách nhiệm gửi email tự động cho người tham gia.

Giải pháp sử dụng:

- AWS Lambda
- Amazon SES
- Amazon EventBridge Scheduler

Lambda xử lý nội dung email, Amazon SES thực hiện việc gửi email, còn EventBridge Scheduler giúp tự động gửi email nhắc lịch đúng thời điểm mà không cần một máy chủ chạy liên tục.

Toàn bộ trạng thái gửi email đều được lưu lại trong DynamoDB để phục vụ việc theo dõi và xử lý sự cố khi cần.

---

## Analytics Dashboard

Hệ thống cũng cung cấp Dashboard giúp quản trị viên theo dõi tình trạng hoạt động của sự kiện.

Lambda sẽ tổng hợp dữ liệu từ DynamoDB để tạo các chỉ số như:

- Tổng số sự kiện
- Tổng số lượt đăng ký
- Số vé đã xác nhận
- Danh sách chờ
- Tỷ lệ tham dự

Cách triển khai này phù hợp với quy mô hiện tại của dự án và vẫn đảm bảo khả năng mở rộng trong tương lai.

---

## Monitoring và Deployment

Tất cả các hàm Lambda đều được cấu hình ghi log lên **Amazon CloudWatch** để theo dõi:

- Thời gian thực thi
- Số lượng request
- Tỷ lệ lỗi
- Exception phát sinh

CloudWatch Alarm có thể kết hợp với Amazon SNS để gửi cảnh báo khi hệ thống gặp sự cố.

Đối với Frontend, nhóm triển khai trên **Amazon S3** kết hợp với **Amazon CloudFront** nhằm tăng tốc độ truy cập và giảm chi phí vận hành so với mô hình sử dụng máy chủ truyền thống.

---

## Những điều mình học được

Qua quá trình thực hiện module này, mình nhận ra rằng phát triển ứng dụng Serverless không chỉ đơn giản là viết Lambda.

Việc thiết kế kiến trúc Event-Driven, xây dựng hệ thống giám sát, tự động hóa quy trình gửi thông báo và triển khai ứng dụng cũng quan trọng không kém để tạo nên một hệ thống Cloud hoạt động ổn định.

Đây cũng là cơ hội giúp mình hiểu rõ hơn cách các dịch vụ AWS phối hợp với nhau để xây dựng một hệ thống có khả năng mở rộng, dễ bảo trì và tối ưu chi phí.

---

## Kiến trúc hệ thống

![Notification & Analytics Architecture](/images/3-Blogs/blog1-architecture.jpg)

---

## Tài liệu tham khảo

- https://aws.amazon.com/lambda/
- https://aws.amazon.com/eventbridge/
- https://aws.amazon.com/ses/
- https://aws.amazon.com/cloudwatch/
- https://aws.amazon.com/serverless/