# AWS VPC Peering Project

## Project Overview

This project demonstrates secure private communication between two Amazon Virtual Private Clouds (VPCs) using VPC Peering in AWS.

The architecture uses a bastion host in VPC A to provide controlled SSH access to two private EC2 application instances. The application instances communicate through private IPv4 addresses across the VPC peering connection.

## Objectives

* Create two VPCs with non-overlapping CIDR blocks.
* Configure public and private subnets.
* Deploy a bastion host in a public subnet.
* Deploy application instances in private subnets.
* Establish a VPC peering connection.
* Configure route tables for communication in both directions.
* Apply security group rules that restrict SSH and ICMP access.
* Verify forward and reverse connectivity.
* Demonstrate controlled failure and recovery by modifying an ICMP security group rule.

## Architecture

**VPC A — `10.10.0.0/16`**

* Public subnet: `10.10.1.0/24`
* Private subnet: `10.10.2.0/24`
* Bastion host: `10.10.1.10`
* App-A: `10.10.2.10`

**VPC B — `10.20.0.0/16`**

* Private subnet: `10.20.1.0/24`
* App-B: `10.20.1.10`

The VPCs are connected through an active VPC peering connection. Route tables direct traffic for the remote VPC CIDR through the peering connection.

The bastion host provides SSH access to the private application instances. App-A and App-B do not have public IPv4 addresses.

## AWS Services Used

* Amazon VPC
* VPC Peering
* Amazon EC2
* Internet Gateway
* Route Tables
* Public and Private Subnets
* Security Groups
* SSH
* ICMP

## Security Configuration

* SSH access to the bastion is restricted to a specific public IPv4 address using a `/32` source.
* SSH access to the application instances is restricted to the bastion's private IPv4 address.
* ICMP Echo Request rules permit the required application-to-application connectivity.
* App-A and App-B do not have public IPv4 addresses.
* Security group rules were tested by temporarily removing and restoring the required ICMP rule.

## Testing and Results

| Test                                            | Result                                               |
| ----------------------------------------------- | ---------------------------------------------------- |
| App-A to App-B ping                             | Successful: 4 packets received, 0% packet loss       |
| App-B to App-A ping                             | Successful: 4 packets received, 0% packet loss       |
| Controlled failure                              | Ping failed after the relevant ICMP rule was removed |
| Recovery                                        | Ping succeeded after the ICMP rule was restored      |
| SSH to App-B through the bastion                | Successful                                           |
| Public IPv4 assignment on application instances | None                                                 |

The initial baseline ping test before peering was configured was not captured. The controlled-failure test was captured later by temporarily removing the relevant security group rule.

## Evidence

Screenshots documenting the configuration and test results are available in the [`screenshots/AWS_Internship_project`](screenshots/AWS_Internship_project/) directory.

The evidence includes:

* VPC and subnet configuration
* Active VPC peering connection
* Route tables
* EC2 instances and private IP addresses
* Security group rules
* SSH connectivity through the bastion
* Forward and reverse ping tests
* Controlled failure and recovery

## Key Learnings

* How VPC peering enables private communication between two VPCs.
* Why non-overlapping CIDR ranges are necessary.
* How route tables direct traffic between VPCs.
* How security groups control inbound traffic.
* Why both forward and return routes are important.
* How a bastion host provides controlled SSH access to private instances.
* How security group changes can be used to test network isolation and recovery.

## Environment

* AWS Region: Asia Pacific (Hyderabad), `ap-south-2`
* Operating System: Amazon Linux 2023
* Project Type: AWS Cloud Networking Internship Project

## Security Note

Private keys, AWS credentials, passwords, and other secrets are not included in this repository.
