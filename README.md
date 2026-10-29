# Task 29: Create a Private and Public Network Resource on AWS

## 📌 Project Overview

This project demonstrates how to configure public and private network resources using **Amazon Virtual Private Cloud (Amazon VPC)**. The objective is to understand subnet segmentation, Internet Gateway configuration, route tables, and traffic flow within a cloud network.

The environment was created using the AWS Management Console in the Asia Pacific (Mumbai) region (`ap-south-1`).

## 🎯 Objectives

- Understand the fundamentals of Amazon VPC and subnetting.
- Create public and private subnets within a custom VPC.
- Configure an Internet Gateway for public internet routing.
- Create separate route tables for public and private subnets.
- Understand routing behavior and basic network isolation.
- Document the architecture and traffic paths.

## 🛠️ Technologies and Services

- Amazon VPC
- Public and private subnets
- Internet Gateway (IGW)
- Route tables
- AWS Management Console

## 🏗️ Network Architecture

The network consists of a custom VPC containing two subnets with separate route tables.

```text
                         INTERNET
                            |
                            |
                    Internet Gateway
                    veda-task29-igw
                            |
              +-------------+-------------+
              |                           |
              |       Custom VPC          |
              |   10.20.0.0/16            |
              |                           |
              |   +-------------------+   |
              |   |   Public Subnet   |   |
              |   |   10.20.1.0/24    |   |
              |   |                   |   |
              |   | Public Route Table|   |
              |   | 0.0.0.0/0 -> IGW  |   |
              |   +-------------------+   |
              |                           |
              |   +-------------------+   |
              |   |  Private Subnet   |   |
              |   |   10.20.2.0/24    |   |
              |   |                   |   |
              |   | Private Route     |   |
              |   | Local route only  |   |
              |   +-------------------+   |
              |                           |
              +---------------------------+
```

## 🌐 Network Configuration

### 1. VPC Configuration

| Parameter | Value |
|---|---|
| VPC Name | `veda-task29-vpc` |
| VPC ID | `vpc-07f2e6b64f9672065` |
| IPv4 CIDR | `10.20.0.0/16` |
| AWS Region | Mumbai (`ap-south-1`) |
| DNS Resolution | Enabled |

### 2. Subnet Configuration

| Parameter | Public Subnet | Private Subnet |
|---|---|---|
| Name | `veda-task29-public-subnet` | `veda-task29-private-subnet` |
| Subnet ID | `subnet-01bd45d9852f4af99` | `subnet-0761a3eb298699c1d` |
| IPv4 CIDR | `10.20.1.0/24` | `10.20.2.0/24` |
| Availability Zone | `ap-south-1a` | `ap-south-1a` |
| Auto-assign Public IPv4 | Enabled | Disabled |
| State | Available | Available |

### 3. Internet Gateway

| Parameter | Value |
|---|---|
| Name | `veda-task29-igw` |
| Internet Gateway ID | `igw-0123718c2c03f9819` |
| Attached VPC | `veda-task29-vpc` |
| State | Attached |

### 4. Route Table Configuration

**Public Route Table**

| Parameter | Value |
|---|---|
| Name | `veda-task29-public-rt` |
| Route Table ID | `rtb-00c275e3f674d7cd8` |
| Associated Subnet | `veda-task29-public-subnet` |

Routes:

| Destination | Target | Purpose |
|---|---|---|
| `10.20.0.0/16` | `local` | Communication within the VPC |
| `0.0.0.0/0` | Internet Gateway | Route for external IPv4 destinations |

**Private Route Table**

| Parameter | Value |
|---|---|
| Name | `veda-task29-private-rt` |
| Route Table ID | `rtb-0f51ce6d850be8bfb` |
| Associated Subnet | `veda-task29-private-subnet` |

Routes:

| Destination | Target | Purpose |
|---|---|---|
| `10.20.0.0/16` | `local` | Communication within the VPC |

The private route table has no direct route to an Internet Gateway.

## 🔄 Traffic Flow Explanation

### Public Subnet

1. Traffic originating outside the VPC and destined for a public resource can reach it through the Internet Gateway when the resource has a public IPv4 address and its security controls permit the traffic.
2. The public subnet's route table directs external IPv4 traffic (`0.0.0.0/0`) to the Internet Gateway.
3. The local route allows traffic between addresses within the VPC.

### Private Subnet

1. The private subnet uses a separate route table.
2. Its local route supports communication within the VPC.
3. It has no direct Internet Gateway route, and automatic public IPv4 assignment is disabled.
4. A resource in this subnet does not gain direct internet access from this configuration. Outbound internet access, if needed, requires an appropriate design such as a NAT Gateway or NAT instance.

## 🔐 Security Considerations

- Use separate route tables to control network traffic.
- Avoid assigning public IP addresses to private resources.
- Restrict inbound and outbound traffic with appropriate security groups and network ACLs.
- Expose only the resources and ports required by the application.
- Review AWS resource usage and remove unused resources after completing the task.
- An Internet Gateway route alone does not make a resource reachable; public addressing and security rules also matter.

## 📸 Project Evidence

Add your AWS Console screenshots to a folder named `screenshots/`.

Suggested evidence:

- VPC creation and details
- Public subnet configuration
- Private subnet configuration
- Internet Gateway attachment
- Public route table and routes
- Public subnet route table association
- Private route table and routes
- Private subnet route table association
- Public IPv4 assignment setting
- Final subnet and routing verification

## 📚 Key Learnings

- A VPC provides an isolated virtual network in AWS.
- Subnets divide a VPC's IP address range into smaller network segments.
- A public subnet uses a route to an Internet Gateway for internet routing.
- A private subnet can be configured without a direct Internet Gateway route.
- Route tables determine how network traffic is directed.
- Public IPv4 assignment, routing, and security rules work together to determine accessibility.

## ⚠️ Scope and Validation

This task demonstrates the network configuration through AWS Console settings. No EC2 instance was launched as part of this implementation, so actual application connectivity and end-to-end traffic were not tested.

The public route and subnet association were verified in the AWS Console. The private route table was configured with only the local VPC route.
