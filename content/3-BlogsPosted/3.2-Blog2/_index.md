---
title: "Blog 2"
date: 2026-07-01
weight: 2
chapter: false
pre: "<b> 3.2. </b>"
---

# Building a Hybrid Multi-Tenant SaaS Architecture for Stateful Services on AWS

## Introduction

Designing a **Multi-Tenant SaaS** architecture is one of the most important challenges when building cloud-native applications. This becomes even more complex for **stateful services**, such as game servers, real-time chat applications, and collaborative platforms, where user sessions and application states must be maintained continuously.

In this AWS Architecture Blog, I learned about the **Hybrid Multi-Tenant Architecture**, an approach that combines the advantages of both the **Shared (Pool)** and **Dedicated (Silo)** deployment models to achieve a balance between cost efficiency, scalability, and performance.

---

## Architecture Overview

![Hybrid Multi-Tenant SaaS Architecture](../../images/3-Blogs/hybrid-multitenant.jpg)

The architecture consists of three main layers:

- **Central Routing Layer**
- **Infrastructure Layer**
- **State Management Layer**

Instead of deploying every customer on the same infrastructure, each **Tenant** is dynamically routed to the most appropriate infrastructure group based on predefined routing policies.

---

## How the Architecture Works

### Step 1. Tenant Request

Every tenant accesses the application through a single **Unified Endpoint**.

Each request contains a **Tenant ID**, which is usually included in the HTTP header or authentication token.

---

### Step 2. Centralized Routing

**Amazon Route 53** manages DNS resolution and forwards requests to the appropriate infrastructure group.

Traffic routing can be based on:

- Header-based routing
- Weighted routing
- Traffic distribution policies

Meanwhile, **Amazon CloudWatch Dashboards** continuously collect metrics from the infrastructure to monitor system health and performance.

---

### Step 3. Infrastructure Selection

Once the tenant has been identified, the request is forwarded to the corresponding **Infrastructure Group**.

Each Infrastructure Group contains:

- Application Load Balancer
- Amazon EKS Cluster
- Multiple Kubernetes Pods

Depending on business requirements:

- Small customers share the same infrastructure to reduce costs.
- Enterprise customers receive dedicated infrastructure for better isolation and performance.

---

### Step 4. Stateful Session Management

For stateful applications, maintaining user sessions is critical.

**Amazon ElastiCache for Redis** is used to:

- Store session data
- Cache frequently accessed information
- Synchronize application state across multiple Pods

As a result, if one Pod becomes unavailable, users can continue their sessions without interruption.

---

## Hybrid Multi-Tenant Model

This architecture combines two common SaaS deployment models.

### Pool Model

**Advantages**

- Lower infrastructure cost
- Better resource utilization
- Suitable for small and medium customers

**Disadvantages**

- Risk of the **Noisy Neighbor** problem
- Performance may be affected by other tenants

---

### Silo Model

**Advantages**

- Dedicated infrastructure for each tenant
- Better security and performance isolation
- Ideal for enterprise customers

**Disadvantages**

- Higher operational cost
- Lower infrastructure utilization

---

### Hybrid Model

The Hybrid approach combines the strengths of both models.

- Small tenants share infrastructure resources.
- Enterprise tenants receive dedicated infrastructure.
- Customers can be migrated between deployment models without modifying application code.

This flexibility is one of the biggest advantages of the architecture.

---

## AWS Services Used

| AWS Service | Role |
|---|---|
| Amazon Route 53 | Manages DNS and routes tenant requests to the correct infrastructure group. |
| Application Load Balancer | Distributes incoming traffic across application workloads. |
| Amazon EKS | Runs containerized applications and isolates tenants using Kubernetes. |
| Amazon ElastiCache for Redis | Stores session data, caches information, and synchronizes application state between Pods. |
| AWS PrivateLink | Provides secure private connectivity between AWS services and VPCs. |
| Amazon CloudWatch | Collects metrics, monitors system health, and provides operational dashboards. |

---

## Benefits of the Architecture

- Balances infrastructure cost and application performance.
- Supports large-scale multi-tenant SaaS platforms.
- Makes it easy to upgrade customers from shared to dedicated infrastructure.
- Centralizes tenant management on a single Kubernetes platform.
- Provides low-latency state synchronization for stateful workloads.

---

## Personal Reflection

The most interesting aspect of this architecture is its **decoupled routing layer**.

Instead of permanently assigning a tenant to a specific infrastructure, routing decisions are determined dynamically through configuration. This allows organizations to move customers between infrastructure groups without changing application logic or redeploying services.

I also found Redis to be a critical component for maintaining session consistency across multiple Kubernetes Pods. It ensures users experience minimal disruption even when Pods are restarted or replaced.

However, stateful services still present challenges during **Auto Scaling**. When Kubernetes scales down Pods, user sessions must be migrated safely before a Pod is terminated. Otherwise, active users may lose their connections or application state.

---

## Conclusion

Hybrid Multi-Tenant Architecture provides an effective solution for modern SaaS applications, especially those requiring persistent application state.

By combining shared and dedicated deployment models, organizations can reduce infrastructure costs while maintaining high performance for enterprise customers.

After studying this architecture, I gained a better understanding of how AWS designs scalable SaaS platforms. More importantly, I realized the value of separating the routing layer from the infrastructure layer, making future scaling and infrastructure changes much easier.

---

## References

AWS Architecture Blog.

**Building a Hybrid Multi-Tenant Architecture for Stateful Services on AWS**

https://aws.amazon.com/blogs/architecture/building-hybrid-multi-tenant-architecture-for-stateful-services-on-aws/