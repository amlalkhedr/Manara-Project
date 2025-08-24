## 📌 Project Title
**Secure & Scalable Web Application with ALB and Auto Scaling on AWS**

---

## 🧠 Project Description  

This project showcases the deployment of a **secure**, **highly available**, and **scalable** multi-tier web application architecture on **AWS**.

It utilizes **two Application Load Balancers (ALBs)** to ensure proper traffic distribution and logical tier separation:

- 🌐 A **Web-tier ALB (external/public-facing)** to route client traffic to **Front-End EC2 instances** in private subnets.
- 🔒 An **App-tier ALB (internal)** to securely forward requests from the **Front-End EC2s** to **Back-End EC2 instances**.

The architecture spans **multiple Availability Zones** for fault tolerance. The **Front-End** tier is managed through an **Auto Scaling Group (ASG)**, while the **Back-End** connects to an **Amazon RDS (MySQL)** database deployed in **Multi-AZ** for high availability.

Observability is achieved using **Amazon CloudWatch** for metrics and logs, and **Amazon SNS** for alert notifications. All resources are protected with properly scoped **IAM roles** and **security groups**, following the **principle of least privilege**.

---

## 🏗️ Solution Architecture

![Diagram drawio](https://github.com/user-attachments/assets/7872c418-f59a-4e15-9054-866e30f0f650)



---

## 🧱 VPC & Subnet Design

- **VPC CIDR:** `10.10.0.0/16`
- **Region:** `eu-north-1 (Stockholm)`
- Deployment across **two Availability Zones**: `eu-north-1a` and `eu-north-1b`

### Subnet Allocation

| Subnet Type     | Name              | CIDR Block        | Availability Zone | Purpose                            |
|-----------------|-------------------|-------------------|-------------------|------------------------------------|
| **Public**      | Public-A          | 10.10.1.0/24      | eu-north-1a       | Web-tier ALB + NAT Gateway         |
|                 | Public-B          | 10.10.2.0/24      | eu-north-1b       | Web-tier ALB + NAT Gateway         |
| **Private FE**  | Private-FE-A      | 10.10.101.0/24    | eu-north-1a       | Front-End EC2 (ASG)                |
|                 | Private-FE-B      | 10.10.102.0/24    | eu-north-1b       | Front-End EC2 (ASG)                |
| **Private BE**  | Private-BE-A      | 10.10.201.0/24    | eu-north-1a       | Back-End EC2                       |
|                 | Private-BE-B      | 10.10.202.0/24    | eu-north-1b       | Back-End EC2                       |
| **Private RDS** | Private-RDS-A     | 10.10.251.0/24    | eu-north-1a       | RDS Primary                        |
|                 | Private-RDS-B     | 10.10.252.0/24    | eu-north-1b       | RDS Standby (Multi-AZ)            |

---

## ⚙️ AWS Services Used

### Networking
- **Amazon VPC**, **Subnets**, **Route Tables**, **Internet Gateway**, **NAT Gateway**

### Load Balancers
- **Web-tier ALB (Public)**: Handles internet traffic to **Front-End EC2s**
- **App-tier ALB (Internal)**: Routes internal traffic to **Back-End EC2s**

### Compute
- **Auto Scaling Group** for **Front-End EC2s**
- **EC2 Instances** for both Front-End and Back-End layers

### Database
- **Amazon RDS (MySQL)** in **Multi-AZ** configuration

### Security
- Security Groups:
  - **Web-tier ALB**: Allows HTTP/HTTPS from the internet
  - **Front-End EC2**: Accepts traffic only from Web-tier ALB
  - **App-tier ALB**: Accepts traffic only from Front-End EC2s
  - **Back-End EC2**: Accepts traffic only from App-tier ALB
  - **RDS**: Accepts traffic only from Back-End EC2
- IAM Roles:
  - **FE EC2**: Access to CloudWatch Logs
  - **BE EC2**: Access to RDS, SSM, CloudWatch
  - All roles use **least privilege**

### Monitoring & Alerting
- **Amazon CloudWatch**: Logs, metrics, alarms
- **Amazon SNS**: Email notifications for threshold breaches

---

## 🔐 Security Architecture

| Component         | Ingress Allowed From     | Port(s)         |
|------------------|--------------------------|-----------------|
| Web-tier ALB     | Internet (0.0.0.0/0)     | 80, 443         |
| Front-End EC2    | Web-tier ALB only        | 80, 443         |
| App-tier ALB     | Front-End EC2 SG         | 8080 (or custom)|
| Back-End EC2     | App-tier ALB only        | 8080            |
| Amazon RDS       | Back-End EC2 SG only     | 3306            |

---

## 🧪 Testing & Validation

- ✅ Application accessible via **Web-tier ALB DNS**
- ✅ **Front-End EC2s** scale automatically under load
- ✅ Internal routing from FE → App-tier ALB → BE verified
- ✅ Back-End EC2 → RDS connectivity tested
- ✅ Simulated AZ failover tested for RDS
- ✅ No direct internet access to private subnets
- ✅ NAT Gateway enables outbound access for updates

---

## 🚀 Deployment Flow

1. Create the **VPC**, **subnets**, **IGW**, and **NAT Gateway**
2. Set up **public and private subnets** across two AZs
3. Launch:
   - **Web-tier ALB** in public subnets
   - **App-tier ALB** in private subnets
4. Configure **Target Groups** and **Listeners**
5. Deploy **Launch Templates** and **Auto Scaling Groups** for FE EC2s
6. Launch BE EC2s and deploy app logic
7. Set up **Amazon RDS MySQL** in Multi-AZ
8. Configure **IAM roles**, **Security Groups**, and **Routing**
9. Enable **CloudWatch alarms** and **SNS notifications**

---

## 📚 Learning Outcomes

- Design scalable multi-tier applications on AWS
- Separate tiers with external/internal ALBs
- Secure resources with least-privilege IAM & SGs
- Implement high availability using Auto Scaling & Multi-AZ RDS
- Set up proactive monitoring and alerting mechanisms
- Deploy real-world VPC architectures using best practices


