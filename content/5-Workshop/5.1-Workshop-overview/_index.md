---
title : "Introduction"
date : 2026-06-28
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Project Overview

The **AWS Event Management Platform** is a cloud-native web application developed to simplify and automate the event management process. The platform enables organizers to create and manage events, while participants can register online, receive QR-code tickets, check in during the event and obtain participation certificates after completing attendance.

The project adopts a **Serverless Architecture** on Amazon Web Services (AWS) to improve scalability, reduce infrastructure management and optimize operational costs. Instead of maintaining traditional application servers, the platform relies on managed AWS services that automatically scale based on workload.

#### Solution Overview

The proposed solution consists of several core components working together to provide a complete event management workflow.

- **Amazon S3** hosts the React frontend application.
- **Amazon CloudFront** distributes static content globally with low latency.
- **Amazon Cognito** provides user authentication and authorization.
- **Amazon API Gateway** exposes REST APIs for frontend communication.
- **AWS Lambda** executes backend business logic using a serverless computing model.
- **Amazon DynamoDB** stores event information, registrations, tickets and attendance records.
- **Amazon SES** sends email notifications such as registration confirmation and event reminders.
- **Amazon CloudWatch** monitors system performance and collects application logs.

This architecture allows each component to operate independently while remaining loosely coupled, making the system easier to maintain and extend in the future.

#### Workshop Overview

In this workshop, you will build the core components of the AWS Event Management Platform step by step using AWS managed services.

The workshop includes:

- Building the frontend application.
- Implementing user authentication with Amazon Cognito.
- Creating REST APIs using Amazon API Gateway.
- Developing backend functions with AWS Lambda.
- Designing data storage using Amazon DynamoDB.
- Sending notification emails with Amazon SES.
- Monitoring the application using Amazon CloudWatch.
- Deploying the serverless application with AWS SAM CLI.

After completing the workshop, participants will understand how to combine multiple AWS services to develop a complete serverless event management application while following AWS best practices for scalability, security and cost optimization.

![AWS Event Management Architecture](/images/5-Workshop/5.1-Introduction/aws-event-architecture.png)