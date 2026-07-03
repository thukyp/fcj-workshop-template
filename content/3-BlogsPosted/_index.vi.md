---
title: "Các bài Blog đã đăng"
date: 2026-06-30
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

## Giới thiệu

Trong quá trình tham gia chương trình **AWS First Cloud Journey**, bên cạnh việc thực hành các bài Lab và phát triển dự án **AWS Event Management Platform**, mình cũng chủ động tìm hiểu thêm thông qua tài liệu chính thức của AWS và các bài viết trên **AWS Architecture Blog**.

Để hệ thống lại kiến thức đã học, mình đã thực hiện một số bài viết kỹ thuật nhằm ghi chép lại những nội dung quan trọng, đồng thời chia sẻ những kinh nghiệm và góc nhìn cá nhân trong quá trình nghiên cứu cũng như triển khai thực tế.

Các bài viết tập trung vào những chủ đề như **Serverless Architecture**, **Hybrid Multi-Tenant SaaS**, và **AWS Lambda Concurrency**. Đây đều là những kiến thức giúp mình hiểu rõ hơn về cách AWS thiết kế các hệ thống có khả năng mở rộng, tính sẵn sàng cao và tối ưu chi phí vận hành.

---

## 📌 [Blog 1 - Building a Serverless Notification & Analytics Architecture on AWS](3.1-Blog1/)

Bài viết giới thiệu kiến trúc Serverless được áp dụng trong dự án **AWS Event Management Platform**, tập trung vào hai thành phần chính là **Notification** và **Analytics**.

Nội dung trình bày cách kết hợp các dịch vụ như AWS Lambda, Amazon SES, Amazon EventBridge Scheduler, Amazon DynamoDB, Amazon CloudWatch, Amazon S3 và Amazon CloudFront để xây dựng hệ thống gửi thông báo tự động, thu thập số liệu thống kê và triển khai ứng dụng mà không cần quản lý máy chủ. Đồng thời, bài viết cũng chia sẻ những kinh nghiệm mình rút ra trong quá trình phát triển dự án thực tế.

---

## 📌 [Blog 2 - Building a Hybrid Multi-Tenant SaaS Architecture for Stateful Services on AWS](3.2-Blog2/)

Bài viết tổng hợp nội dung từ AWS Architecture Blog về mô hình **Hybrid Multi-Tenant SaaS Architecture** dành cho các ứng dụng có trạng thái như Game Server hoặc hệ thống Chat thời gian thực.

Thông qua bài viết, mình tìm hiểu cách kết hợp giữa mô hình **Shared (Pool)** và **Dedicated (Silo)** nhằm cân bằng giữa hiệu năng, khả năng mở rộng và chi phí vận hành. Ngoài ra, bài viết cũng giới thiệu vai trò của Amazon Route 53, Amazon EKS, Amazon ElastiCache for Redis, AWS PrivateLink và Amazon CloudWatch trong việc xây dựng một hệ thống SaaS hiện đại trên AWS.

---

## 📌 [Blog 3 - Deep Dive into AWS Lambda Concurrency](3.3-Blog3/)

Bài viết phân tích sâu hơn về cơ chế hoạt động bên trong của **AWS Lambda**, đặc biệt là khả năng xử lý nhiều request đồng thời thông qua **Execution Environment** và **Concurrency**.

Nội dung giải thích các khái niệm như **Cold Start**, **Warm Start**, **Reserved Concurrency** và **Provisioned Concurrency**, đồng thời trình bày cách Lambda tự động mở rộng khi lưu lượng truy cập tăng cao. Bài viết cũng chia sẻ một số kinh nghiệm thực tế trong quá trình phát triển ứng dụng Serverless và cách sử dụng Amazon CloudWatch để theo dõi hiệu năng của Lambda.

---

Thông qua việc nghiên cứu và viết các bài blog này, mình không chỉ củng cố kiến thức về các dịch vụ AWS mà còn rèn luyện kỹ năng đọc hiểu tài liệu kỹ thuật, phân tích kiến trúc hệ thống và trình bày các chủ đề chuyên môn theo cách dễ tiếp cận hơn. Đây cũng là một phần quan trọng giúp mình kết nối giữa kiến thức lý thuyết và quá trình triển khai dự án thực tế trên nền tảng AWS.