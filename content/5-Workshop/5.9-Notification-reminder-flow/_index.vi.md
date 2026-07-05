---
title: "Thông báo, nhắc lịch tự động & Phân tích dữ liệu"
date: 2026-06-29
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---

---

Phần này trình bày hai module do cùng một thành viên thực hiện:

- **NotificationFunction** — gửi email xác nhận đăng ký và nhắc lịch tự động qua Amazon SES
- **AnalyticsFunction** — thống kê tổng quan toàn hệ thống và chi tiết từng sự kiện

Cả hai module đều được khai báo trong file template hạ tầng và deploy tự động qua SAM, không tạo thủ công trên AWS Console.

---

# PHẦN 1: Thông báo & nhắc lịch tự động

---

Phần này demo hai luồng thông báo độc lập, đều do một Lambda duy nhất (NotificationFunction) xử lý, nhưng được kích hoạt theo hai cơ chế khác nhau và tự phân loại dựa trên cấu trúc JSON của sự kiện đầu vào — không cần route riêng cho từng loại trigger.

Nghiệp vụ này đọc dữ liệu từ các bảng Events, Tickets, Users, ghi log vào bảng Notification Log, và gửi email qua Amazon SES.

---

## Điều kiện tiên quyết

Trước khi test bất kỳ luồng nào, cần đảm bảo email gửi đã được verify trong Amazon SES. Nếu email chưa được verify, toàn bộ luồng gửi mail sẽ thất bại với lỗi MessageRejected từ SES, bất kể code đúng hay sai.

Kiểm tra trạng thái verify:

1. Vào `AWS Console → Amazon SES` → Verified identities
2. Xác nhận địa chỉ khacanh204@gmail.com có trạng thái Verified
3. Nếu chưa verify: chọn Create identity → Email address, nhập email, AWS sẽ gửi link xác nhận vào hộp thư — bấm link đó để hoàn tất

> ⚠️ **Lưu ý:** Nếu tài khoản AWS đang ở chế độ Sandbox (mặc định), SES chỉ cho phép gửi đến các email đã được verify. Cả email gửi (khacanh204@gmail.com) lẫn email nhận đều phải được verify trong Sandbox. Để gửi đến bất kỳ email nào, cần request Production Access trong SES.

---

## Cơ chế phân loại sự kiện đầu vào

Hàm xử lý chính nhận input dạng JSON và tự xác định nguồn gọi dựa trên cấu trúc:

```text
Có trường "httpMethod"        → request từ API Gateway (GET /admin/notifications)
Có trường "detail-type"
  = "Scheduled Event"        → do EventBridge Scheduler gọi (luồng nhắc lịch)
  = "TicketRegistered"       → do EventBridge Rule gọi (luồng xác nhận đăng ký)
```

Đây là một cách thiết kế gộp 3 loại trigger (API, custom event, scheduled event) vào cùng một Lambda handler, thay vì tách thành 3 Lambda riêng.

---

## Luồng 1: Gửi email xác nhận khi đăng ký vé

1. Sau khi hàm đăng ký vé tạo vé thành công (mục 5.7), nó phát một sự kiện lên default event bus với source là eventmanagement.ticket, detail-type là TicketRegistered.
2. Rule EventBridge khớp pattern này sẽ tự động invoke NotificationFunction, truyền cả phần detail chứa thông tin vé (email, họ tên, tiêu đề sự kiện, thời gian, địa điểm).
3. Hàm xử lý luồng này đọc dữ liệu detail, dựng nội dung email xác nhận đăng ký, gửi qua SES từ địa chỉ khacanh204@gmail.com.
4. Kết quả gửi (thành công hay thất bại) được ghi vào bảng Notification Log với loại là RegistrationConfirmed, trạng thái Sent hoặc Failed kèm nội dung lỗi nếu có.

---

## Luồng 2: Gửi email nhắc lịch trước sự kiện

1. EventBridge Scheduler (chạy mỗi giờ một lần) tự động invoke NotificationFunction với một sự kiện dạng "Scheduled Event".
2. Hàm xử lý nhắc lịch quét bảng Events để tìm các sự kiện có thời gian bắt đầu nằm trong khoảng 24 giờ tới kể từ thời điểm hiện tại.
3. Với mỗi sự kiện sắp diễn ra, Lambda lấy danh sách vé đã Confirmed của sự kiện đó.
4. Với từng vé, Lambda dựng nội dung nhắc lịch và gửi email tới địa chỉ email tương ứng của người dùng.
5. Mỗi lần gửi đều được ghi log riêng vào bảng Notification Log với loại là EventReminder.

> ⚠️ **Lưu ý về timeout:** Lambda được cấu hình timeout 30 giây. Nếu số lượng vé Confirmed lớn, luồng nhắc lịch có thể bị timeout trước khi gửi hết email. Trong môi trường test với ít dữ liệu thì không ảnh hưởng.

> ⚠️ **Lưu ý về tần suất:** Vì chạy mỗi giờ một lần, một sự kiện có thể nhận nhắc lịch nhiều lần nếu vẫn còn nằm trong cửa sổ 24 giờ ở các lần quét liên tiếp — không nên kỳ vọng chỉ có đúng 1 email nhắc lịch cho mỗi vé.

---

## Bước 1: Test luồng xác nhận đăng ký (qua đăng ký vé thật)

Đăng ký một vé mới theo đúng các bước ở mục 5.7:

```powershell
Invoke-RestMethod -Uri "$baseUrl/events/$eventId/register" -Method Post -Headers $headers
```

Sau vài giây (do luồng chạy bất đồng bộ qua EventBridge), kiểm tra log thông báo:

```powershell
Invoke-RestMethod -Uri "$baseUrl/admin/notifications" -Method Get -Headers $headers
```

Response mong đợi (rút gọn, chỉ hiển thị bản ghi mới nhất):

```json
{
  "notificationId": "<GUID tự sinh cho mỗi lần gửi>",
  "eventId": "<EventId của sự kiện vừa đăng ký>",
  "email": "<Email của User vừa đăng ký>",
  "type": "RegistrationConfirmed",
  "status": "Sent",
  "errorMessage": null,
  "sentAt": "<Thời điểm gửi thực tế, UTC ISO 8601>"
}
```

---

## Bước 2: Kiểm tra EventBridge Rule đã invoke NotificationFunction

Mở `Amazon EventBridge → Rules`, chọn rule lắng nghe sự kiện đăng ký vé, tab Monitoring, xác nhận số lượt Invocations tăng thêm đúng 1 sau lần đăng ký ở Bước 1.

---

## Bước 3: Kiểm tra log Lambda cho luồng xác nhận đăng ký

Mở `CloudWatch → Log groups → log group` của NotificationFunction, tìm log stream tại đúng thời điểm test, xác nhận có các dòng log tương ứng với luồng xử lý đăng ký vé và kết quả gọi SES.

---

## Bước 4: Test luồng nhắc lịch bằng cách giả lập Scheduled Event

Vì luồng nhắc lịch chỉ tự chạy mỗi giờ, có thể chủ động test bằng cách invoke trực tiếp Lambda với payload giả lập đúng cấu trúc mà EventBridge Scheduler gửi.

Tạo file test-reminder.json với nội dung:

```json
{
  "version": "0",
  "id": "test-event-id",
  "source": "aws.scheduler",
  "account": "<AccountId>",
  "region": "ap-southeast-2",
  "detail-type": "Scheduled Event",
  "detail": {}
}
```

Sau đó invoke Lambda bằng AWS CLI:

```powershell
aws lambda invoke `
  --function-name NotificationLambda `
  --payload file://test-reminder.json `
  --cli-binary-format raw-in-base64-out `
  response.json

Get-Content response.json
```

> ⚠️ **Lưu ý:** Dùng file JSON thay vì inline string để tránh lỗi encoding trên PowerShell. Để có dữ liệu nhắc lịch thật sự xuất hiện, cần đảm bảo có ít nhất một sự kiện với thời gian bắt đầu nằm trong 24 giờ tới và đã có vé Confirmed.

---

## Bước 5: Kiểm tra kết quả luồng nhắc lịch

```powershell
Invoke-RestMethod -Uri "$baseUrl/admin/notifications" -Method Get -Headers $headers
```

Xác nhận xuất hiện thêm các bản ghi mới với loại là EventReminder, số lượng bản ghi bằng đúng số vé Confirmed của các sự kiện sắp diễn ra trong 24 giờ tới.

---

## Bước 6: Kiểm tra dữ liệu trong DynamoDB

Mở `DynamoDB → Tables → bảng Notification Log → Explore table items`, đối chiếu trực tiếp các item với response ở Bước 1 và Bước 5.

---

## Kết quả mong đợi (Phần 1)

Sau khi hoàn thành phần Notification, người thực hiện có thể:

- Hiểu cách một Lambda duy nhất phân loại và xử lý 3 loại trigger khác nhau (API Gateway, EventBridge custom event, EventBridge Scheduler).
- Xác nhận email khacanh204@gmail.com đã được verify trong SES và có thể gửi mail thành công.
- Test được luồng gửi email xác nhận đăng ký, xác nhận qua log thông báo và email thật nhận được trong hộp thư.
- Test được luồng gửi email nhắc lịch bằng cách giả lập Scheduled Event qua AWS CLI với file JSON.
- Đối chiếu dữ liệu log thông báo giữa API response và item thật trong DynamoDB.
- Xác nhận số lượt invoke của EventBridge Rule khớp với số lần đăng ký vé đã test.

---

# PHẦN 2: Phân tích & thống kê dữ liệu

---

Phần này demo hai luồng thống kê độc lập, đều do một Lambda duy nhất (AnalyticsFunction) xử lý, được kích hoạt qua API Gateway và tự định tuyến dựa trên tham số đường dẫn.

Nghiệp vụ này đọc dữ liệu từ các bảng Events, Tickets, Attendance, Notification Log, và S3 bucket chứng chỉ — tổng hợp thành hai dạng báo cáo: tổng quan toàn hệ thống và chi tiết từng sự kiện.

---

## Cơ chế định tuyến

Hàm xử lý chính nhận request từ API Gateway và tự xác định luồng xử lý dựa trên tham số đường dẫn:

```text
Không có tham số đường dẫn "eventId"   → xử lý luồng Dashboard (tổng quan toàn hệ thống)
Có tham số đường dẫn "eventId"         → xử lý luồng phân tích chi tiết từng sự kiện
```

---

## Luồng 1: Thống kê tổng quan toàn hệ thống

Hàm xử lý Dashboard tổng hợp các chỉ số từ nhiều bảng và S3, trả về các thông tin gồm:

- Tổng số sự kiện — đếm bằng cách Scan với chế độ chỉ đếm (Count) trên bảng Events
- Tổng đăng ký, đã xác nhận, đang chờ — scan toàn bộ bảng Tickets theo từng trang (pagination), phân loại theo trạng thái: Waiting, Cancelled (không tính), còn lại là Confirmed
- Tổng số lượt check-in — đếm bằng cách Scan với chế độ chỉ đếm trên bảng Attendance
- Tỷ lệ điểm danh trung bình — tính bằng số check-in chia cho số đã xác nhận, nhân 100, làm tròn 1 chữ số thập phân
- Số email đã gửi và số email lỗi — scan bảng Notification Log theo trạng thái Sent hoặc Failed
- Số chứng chỉ đã cấp — đếm file PDF thực tế trong S3 theo tiền tố certificates/, không đọc từ DynamoDB để đảm bảo chính xác

---

## Luồng 2: Thống kê chi tiết từng sự kiện

Hàm xử lý phân tích theo sự kiện nhận eventId từ tham số đường dẫn, kiểm tra sự kiện tồn tại rồi trả về các thông tin gồm:

- Tổng đăng ký, số đã xác nhận, số đang chờ — scan bảng Tickets với điều kiện lọc theo EventId
- Số lượt check-in — dùng Query (không phải Scan) trên bảng Attendance với EventId là Partition Key — hiệu quả hơn Scan vì không quét toàn bảng
- Tỷ lệ điểm danh của riêng sự kiện đó — tính bằng số check-in chia cho số đã xác nhận, nhân 100

---

## Bước 1: Test API tổng quan Dashboard

```powershell
Invoke-RestMethod -Uri "$baseUrl/admin/analytics/dashboard" -Method Get -Headers $headers
```

Response mong đợi:

```json
{
  "totalEvents": 5,
  "totalRegistrations": 20,
  "confirmedRegistrations": 18,
  "waitingRegistrations": 2,
  "totalCheckIns": 10,
  "averageAttendanceRate": 55.6,
  "emailSentCount": 18,
  "emailFailedCount": 0,
  "certificatesIssuedCount": 10
}
```

---

## Bước 2: Test API thống kê chi tiết từng sự kiện

```powershell
Invoke-RestMethod -Uri "$baseUrl/admin/analytics/events/$eventId" -Method Get -Headers $headers
```

Response mong đợi:

```json
{
  "eventId": "<EventId của sự kiện>",
  "eventTitle": "<Tên sự kiện>",
  "totalRegistrations": 5,
  "confirmedCount": 5,
  "waitingCount": 0,
  "checkInCount": 3,
  "attendanceRate": 60.0
}
```

---

## Bước 3: Đối chiếu dữ liệu với DynamoDB

Mở `DynamoDB → Tables`, lần lượt kiểm tra:

- Bảng Tickets — đếm số item có trạng thái Confirmed và Waiting
- Bảng Attendance — đếm số item, so sánh với tổng số check-in trong Dashboard
- Bảng Notification Log — đếm số item có trạng thái Sent và Failed

Xác nhận các con số khớp với response API ở Bước 1 và Bước 2.

---

## Bước 4: Kiểm tra log Lambda

Mở `CloudWatch → Log groups → log group` của AnalyticsFunction, tìm log stream tại thời điểm test, xác nhận không có lỗi và các query/scan chạy thành công.

---

## Kết quả mong đợi (Phần 2)

Sau khi hoàn thành phần Analytics, người thực hiện có thể:

- Hiểu cách một Lambda xử lý 2 loại request khác nhau dựa trên tham số đường dẫn.
- Nắm được sự khác biệt giữa Scan (quét toàn bảng) và Query (dùng key, hiệu quả hơn) trong DynamoDB — cụ thể luồng phân tích theo sự kiện dùng Query cho bảng Attendance thay vì Scan.
- Xác nhận dữ liệu Dashboard khớp với dữ liệu thực tế trong các bảng DynamoDB và S3.
- Xác nhận số chứng chỉ đã cấp được đếm trực tiếp từ S3, không phụ thuộc vào DynamoDB.
- Dữ liệu Analytics đã sẵn sàng để tổng hợp và trình bày trong báo cáo cuối.