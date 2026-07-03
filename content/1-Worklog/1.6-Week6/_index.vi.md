---
title: "Worklog Tuần 6"
date: 2026-05-25
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

{{% notice info %}}
Báo cáo này tổng hợp các nội dung về kho dữ liệu, bộ nhớ đệm và di chuyển cơ sở dữ liệu trên nền tảng AWS trong tuần thứ sáu.
{{% /notice %}}

## Mục tiêu tuần 6

- Tìm hiểu các dịch vụ phân tích dữ liệu trên AWS.
- Làm quen với Amazon Redshift và Amazon ElastiCache.
- Thực hành di chuyển cơ sở dữ liệu bằng AWS Database Migration Service.

---

## Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|---|---|---|---|---|
| 2 | - Tìm hiểu kiến trúc của Amazon Redshift.<br>- Nghiên cứu cơ chế xử lý song song (MPP) và lưu trữ dạng cột (Columnar Storage). | 25/05/2026 | 25/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | - Thực hành truy vấn dữ liệu với Amazon Redshift.<br>- Tìm hiểu Concurrency Scaling và Redshift Spectrum để tối ưu hiệu năng và chi phí. | 26/05/2026 | 26/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - Tìm hiểu Amazon ElastiCache.<br>- So sánh Redis và Memcached.<br>- Thực hành xây dựng cơ chế Cache cho ứng dụng. | 27/05/2026 | 27/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | - Nghiên cứu quy trình Database Migration.<br>- Làm quen với AWS Database Migration Service (DMS).<br>- Tìm hiểu AWS Schema Conversion Tool (SCT). | 28/05/2026 | 29/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | - Thực hành chuyển đổi Schema và di trú dữ liệu từ Oracle sang Amazon Aurora PostgreSQL.<br>- Hoàn thành các bài Lab liên quan đến Redshift, ElastiCache và DMS. | 30/05/2026 | 31/05/2026 | https://cloudjourney.awsstudygroup.com/ |

---

## Kết quả đạt được tuần 6

- Hiểu vai trò của Amazon Redshift trong việc xây dựng hệ thống kho dữ liệu phục vụ phân tích.
- Nắm được cơ chế xử lý song song (MPP) và lưu trữ dạng cột giúp tối ưu hiệu năng truy vấn.
- Biết cách sử dụng Concurrency Scaling và Redshift Spectrum để cải thiện khả năng xử lý dữ liệu.
- Hiểu vai trò của Amazon ElastiCache trong việc giảm tải cho cơ sở dữ liệu và tăng tốc độ truy xuất dữ liệu.
- Phân biệt được hai Engine Redis và Memcached cùng các trường hợp sử dụng phù hợp.
- Làm quen với quy trình di chuyển cơ sở dữ liệu lên AWS bằng Database Migration Service (DMS).
- Tìm hiểu AWS Schema Conversion Tool (SCT) để chuyển đổi Schema giữa các hệ quản trị cơ sở dữ liệu.
- Hoàn thành các bài Lab về Amazon Redshift, Amazon ElastiCache và Database Migration Service.