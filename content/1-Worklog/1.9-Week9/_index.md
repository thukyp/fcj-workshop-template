---
title: "Week 9"
date: 2026-06-15
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

{{% notice info %}}
This report summarizes the development of the Attendance & Certificate module for the AWS Event Management Platform during the ninth week.
{{% /notice %}}

## Week 9 Objectives

- Develop the QR Code check-in feature.
- Design the attendance database.
- Integrate the frontend with backend APIs.
- Deliver the first working version of the attendance workflow.

---

## Tasks Completed This Week

| Day | Tasks | Start Date | End Date | Reference |
|---|---|---|---|---|
| Monday | - Analyzed the Attendance & Certificate module requirements.<br>- Discussed APIs and data models with team members.<br>- Designed the AttendanceTable in Amazon DynamoDB. | 15/06/2026 | 15/06/2026 | AWS Event Management Project |
| Tuesday | - Created the Check-in Lambda function.<br>- Implemented the QR Code validation workflow using Ticket IDs.<br>- Tested the Lambda function with sample DynamoDB data. | 16/06/2026 | 16/06/2026 | AWS Event Management Project |
| Wednesday | - Designed the administrator QR Check-in interface.<br>- Integrated the QR Scanner component.<br>- Tested the basic check-in workflow. | 17/06/2026 | 17/06/2026 | AWS Event Management Project |
| Thursday | - Connected Amazon API Gateway with AWS Lambda.<br>- Tested the API from the frontend.<br>- Improved request and response handling between frontend and backend. | 18/06/2026 | 19/06/2026 | AWS Event Management Project |
| Friday | - Performed end-to-end testing of the check-in process.<br>- Recorded issues and updated the module documentation.<br>- Completed the first functional version of the attendance feature. | 20/06/2026 | 21/06/2026 | AWS Event Management Project |

---

## Week 9 Achievements

- Successfully designed the AttendanceTable in Amazon DynamoDB.
- Developed the AWS Lambda function for QR Code check-in.
- Implemented Ticket ID validation and attendance status updates.
- Built the administrator QR Check-in interface with QR scanning capability.
- Integrated the frontend with AWS Lambda through Amazon API Gateway.
- Successfully tested the initial check-in workflow and documented identified issues.
- Updated the module documentation in preparation for the Certificate feature development in the following week.