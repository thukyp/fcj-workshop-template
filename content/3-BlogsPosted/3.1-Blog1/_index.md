---
title: "Blog 1"
date: 2026-06-30
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Building a Serverless Notification & Analytics Architecture on AWS

During the development of our **AWS Event Management Platform**, I was responsible for implementing the **Notification**, **Analytics**, and **Deployment** modules. Initially, I considered these modules as supporting components, but I soon realized they play an essential role in improving system scalability, observability and operational efficiency.

Instead of deploying traditional application servers, our team adopted a **Serverless Architecture** on AWS. This approach minimizes infrastructure management while allowing the system to scale automatically according to workload.

## Event-Driven Architecture

One of the design goals was to reduce coupling between different modules.

Rather than allowing one service to directly invoke another, the system is designed around **events**. Whenever an important action occurs, such as a successful registration or ticket creation, an event is generated and processed asynchronously.

This architecture allows future modules to be added without changing the existing business logic, making the system easier to maintain and extend.

---

## Notification Service

The notification module is responsible for automatically sending emails to participants.

The implementation combines:

- AWS Lambda
- Amazon SES
- Amazon EventBridge Scheduler

Lambda prepares email content, while Amazon SES handles email delivery. Scheduled reminder emails are triggered automatically by EventBridge Scheduler without requiring a continuously running server.

To support troubleshooting and monitoring, every email status is recorded inside a notification log stored in DynamoDB.

---

## Analytics Dashboard

The project also includes an analytics dashboard for administrators.

Instead of storing statistical values directly, Lambda retrieves and aggregates data from DynamoDB before returning the results.

Some important metrics include:

- Total Events
- Total Registrations
- Confirmed Tickets
- Waiting List
- Attendance Rate

This design keeps the architecture simple while remaining suitable for small and medium-sized workloads.

---

## Monitoring and Deployment

Every Lambda function writes execution logs to **Amazon CloudWatch**, allowing the development team to monitor:

- Lambda execution time
- Request count
- Error rate
- Exception logs

CloudWatch Alarms can also be integrated with Amazon SNS to automatically notify administrators whenever abnormal behavior is detected.

For deployment, the frontend is hosted on **Amazon S3** and distributed globally using **Amazon CloudFront**, providing lower latency and reducing infrastructure costs compared to traditional web servers.

---

## What I Learned

Working on this module helped me understand that serverless applications involve much more than writing Lambda functions.

Designing an event-driven architecture, implementing monitoring, automating notifications and deploying applications efficiently are equally important for building reliable cloud-native systems.

This experience also strengthened my understanding of how AWS managed services can work together to create scalable, cost-effective and maintainable applications.

---

## Architecture

![Notification & Analytics Architecture](/images/3-Blogs/blog1-architecture.jpg)

---

## References

- https://aws.amazon.com/lambda/
- https://aws.amazon.com/eventbridge/
- https://aws.amazon.com/ses/
- https://aws.amazon.com/cloudwatch/
- https://aws.amazon.com/serverless/