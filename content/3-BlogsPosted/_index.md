---
title: "Blogs Posted"
date: 2026-06-30
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

## Overview

During the AWS First Cloud Journey program, I actively explored various AWS services and cloud architecture concepts through technical blog writing. Besides completing hands-on labs, I also spent time studying AWS documentation, AWS Architecture Blog articles, and best practices to gain a deeper understanding of cloud-native application development.

The following blogs summarize the knowledge I learned while researching AWS services and applying them to the **AWS Event Management Platform**. Each article not only explains the underlying concepts but also includes my own observations, practical experiences, and lessons learned throughout the implementation process.

Writing these blogs helped me strengthen my understanding of Serverless Architecture, SaaS design patterns, AWS managed services, and cloud scalability, while also improving my technical writing and knowledge-sharing skills.

---

## 📌 [Blog 1 - Building a Serverless Notification & Analytics Architecture on AWS](3.1-Blog1/)

This article introduces the serverless architecture used in the **AWS Event Management Platform**, focusing on the Notification and Analytics modules.

The blog explains how AWS Lambda, Amazon SES, Amazon EventBridge Scheduler, Amazon DynamoDB, Amazon CloudWatch, Amazon S3, and Amazon CloudFront work together to build a scalable event-driven application with minimal operational overhead. It also summarizes the lessons I learned while implementing asynchronous notifications, monitoring Lambda functions, and deploying a serverless frontend.

---

## 📌 [Blog 2 - Building a Hybrid Multi-Tenant SaaS Architecture for Stateful Services on AWS](3.2-Blog2/)

This blog summarizes an AWS Architecture Blog discussing Hybrid Multi-Tenant SaaS Architecture for stateful services.

The article explains how Shared (Pool) and Dedicated (Silo) deployment models can be combined to balance infrastructure cost, scalability, and performance. It also describes the roles of Amazon Route 53, Amazon EKS, Amazon ElastiCache for Redis, AWS PrivateLink, and Amazon CloudWatch in building a highly available multi-tenant platform. Finally, I share my own perspective on why separating the routing layer from the infrastructure layer provides greater flexibility for future system growth.

---

## 📌 [Blog 3 - Deep Dive into AWS Lambda Concurrency](3.3-Blog3/)

This article explores the internal execution model of AWS Lambda beyond its serverless abstraction.

It covers key concepts including Execution Environments, Cold Starts, Warm Starts, Reserved Concurrency, and Provisioned Concurrency. The blog also explains how Lambda automatically scales to process thousands of concurrent requests and how Amazon CloudWatch can be used to monitor concurrency, throttling, and execution performance. The article concludes with several best practices that I learned while developing serverless applications on AWS.

---

These blogs represent an important part of my learning journey throughout the AWS First Cloud Journey program. They demonstrate not only my understanding of AWS services but also my ability to research technical documentation, analyze cloud architectures, and communicate technical concepts in a structured and practical manner.