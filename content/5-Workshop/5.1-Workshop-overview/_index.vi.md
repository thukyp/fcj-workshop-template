---
title : "Giới thiệu"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Tổng quan dự án

**AWS Event Management Platform** là hệ thống quản lý sự kiện được phát triển trên nền tảng điện toán đám mây Amazon Web Services (AWS), nhằm tự động hóa toàn bộ quy trình tổ chức sự kiện. Hệ thống cho phép ban tổ chức tạo và quản lý sự kiện, trong khi người tham gia có thể đăng ký trực tuyến, nhận vé điện tử QR, thực hiện điểm danh bằng mã QR và nhận chứng nhận tham gia sau khi hoàn thành sự kiện.

Dự án được xây dựng theo kiến trúc **Serverless**, giúp giảm chi phí vận hành, hạn chế việc quản lý máy chủ và tận dụng khả năng tự động mở rộng của các dịch vụ AWS. Nhờ đó, hệ thống có thể đáp ứng số lượng người dùng lớn mà vẫn đảm bảo hiệu năng, tính ổn định và khả năng mở rộng trong tương lai.

#### Tổng quan giải pháp

Giải pháp được xây dựng từ nhiều dịch vụ AWS kết hợp với nhau để tạo thành một hệ thống quản lý sự kiện hoàn chỉnh.

- **Amazon S3** dùng để lưu trữ và triển khai giao diện người dùng (React).
- **Amazon CloudFront** phân phối nội dung thông qua mạng CDN nhằm tăng tốc độ truy cập.
- **Amazon Cognito** quản lý xác thực và phân quyền người dùng.
- **Amazon API Gateway** cung cấp các REST API cho ứng dụng.
- **AWS Lambda** xử lý toàn bộ nghiệp vụ theo mô hình Serverless.
- **Amazon DynamoDB** lưu trữ dữ liệu về sự kiện, người tham gia, vé và thông tin điểm danh.
- **Amazon SES** gửi email xác nhận đăng ký và các thông báo liên quan đến sự kiện.
- **Amazon CloudWatch** giám sát hoạt động của hệ thống và lưu trữ nhật ký (Logs).

Việc sử dụng các dịch vụ quản lý của AWS giúp các thành phần trong hệ thống hoạt động độc lập, dễ bảo trì, dễ mở rộng và tối ưu chi phí vận hành.

#### Tổng quan Workshop

Workshop này sẽ hướng dẫn từng bước xây dựng hệ thống **AWS Event Management Platform** bằng các dịch vụ Serverless trên AWS.

Các nội dung chính bao gồm:

- Xây dựng giao diện người dùng.
- Thiết lập xác thực người dùng với Amazon Cognito.
- Xây dựng REST API bằng Amazon API Gateway.
- Phát triển các chức năng Backend bằng AWS Lambda.
- Thiết kế cơ sở dữ liệu trên Amazon DynamoDB.
- Gửi email tự động bằng Amazon SES.
- Giám sát hệ thống với Amazon CloudWatch.
- Triển khai ứng dụng bằng AWS SAM CLI.

Sau khi hoàn thành Workshop, người thực hiện sẽ hiểu cách kết hợp nhiều dịch vụ AWS để xây dựng một hệ thống quản lý sự kiện theo kiến trúc Serverless, đồng thời nắm được quy trình triển khai, vận hành và giám sát một ứng dụng Cloud theo các khuyến nghị của AWS.

![Kiến trúc AWS Event Management Platform](/images/5-Workshop/5.1-Introduction/aws-event-architecture.png)