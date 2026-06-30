---
title: "Week 9 Worklog"
date: 2026-06-15
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

{{% notice info %}}
This worklog summarizes the implementation of the Attendance module for the AWS Event Management Platform using a serverless architecture.
{{% /notice %}}

### Week 9 Objectives

- Design the attendance management module.
- Implement the QR Check-in workflow.
- Develop backend APIs using AWS Lambda.
- Integrate the frontend with serverless services.

---

### Daily Activities

| Day | Activities |
|-----|------------|
| Monday | Analyzed the Attendance module requirements, discussed the API structure with team members and finalized the attendance workflow. |
| Tuesday | Designed the Attendance table in Amazon DynamoDB, defined partition keys, attributes and data structure for attendance records. |
| Wednesday | Developed the Check-in AWS Lambda function using AWS SAM and implemented the business logic for validating QR codes. |
| Thursday | Configured Amazon API Gateway and integrated it with AWS Lambda to expose attendance APIs for frontend communication. |
| Friday | Developed the administrator QR Check-in interface using React and integrated it with the backend APIs. |
| Saturday | Performed end-to-end testing between React, API Gateway, Lambda and DynamoDB, then fixed issues found during testing. |
| Sunday | Reviewed the attendance workflow, optimized Lambda logic and updated project documentation. |

---

### Hands-on Practice

- Designed the Attendance table in Amazon DynamoDB.
- Developed AWS Lambda functions using AWS SAM CLI.
- Configured Amazon API Gateway REST APIs.
- Integrated React frontend with serverless backend services.
- Used Docker to test Lambda functions locally.
- Stored attendance records in Amazon DynamoDB.
- Tested the complete QR Check-in workflow.

---

### Knowledge Gained

- AWS Lambda.
- Amazon API Gateway.
- Amazon DynamoDB.
- AWS SAM CLI.
- Docker.
- REST API Integration.
- QR Code validation workflow.

---

### Results Achieved

- Successfully completed the first version of the Attendance module.
- Integrated React with API Gateway and AWS Lambda.
- Built a functional QR Check-in workflow.
- Stored attendance information in Amazon DynamoDB.
- Established a stable foundation for implementing certificate generation in the following week.