---
title: AWS
date: 2024-03-09T11:35:50.5050+05:30
draft: false
tags:
  - devops
  - aws
---

## Cell-Based Architecture
https://newsletter.systemdesign.one/p/cell-based-architecture
https://slack.engineering/slacks-migration-to-a-cellular-architecture/


## VPC

**Subnets**: we can create **subnets** within a VPC to divide the network into public and private sections. Public subnets are accessible from the internet, while private subnets are isolated

**Internet Gateways**: Attach an internet gateway to your VPC to enable communication between your instances and the internet.

A VPC spans all **Availability Zones** within a given Region, but individual subnets within the VPC are tied to a specific Availability Zone.

**NAT** stands for **Network Address Translation**. It allows instances in a **private subnet** (which doesn’t have direct internet access) to **outbound traffic to the internet** (e.g., for downloading updates) while preventing incoming traffic initiated from the internet.

AWS offers two primary ways to implement NAT:

1. **NAT Gateway**:
    - Managed service by AWS.
    - Highly available within a single AZ.
    - Scales automatically to handle large amounts of traffic.
    - Best for production environments.
2. **NAT Instance**:
    - A manually managed EC2 instance that you configure to perform NAT functions.
    - Requires manual scaling, maintenance, and availability management.
    - Useful for smaller or more cost-sensitive environments.

 
 **NAT Gateway/Instance**: Allows **outbound** traffic to the internet from private subnets.

 **Internet Gateway**: Enables **bi-directional** traffic (both inbound and outbound) between the internet and resources in **public subnets**.