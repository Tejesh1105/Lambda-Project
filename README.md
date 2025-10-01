## 🛡️ AWS Security Group Compliance Monitor Using AWS Lambda.

![image alt](https://github.com/Tejesh1105/Lambda-Project/blob/369d9ccec6d0ebb0bcb253904cd27182b8290668/Lamda%20Project%20Flow-Chart.PNG)

This project implements an **automated, multi-region AWS security monitoring solution** designed to identify and report on overly permissive **EC2 Security Group (SG) ingress rules** that allow access from the entire internet (`0.0.0.0/0`).

It uses **AWS Lambda** for the core auditing logic, **Amazon EventBridge** for scheduled execution, **Amazon SNS** for immediate alerts, and **Amazon S3** for persistent reporting.

***

## Project Overview

The core purpose of this tool is to enhance the security posture of an AWS environment by automatically enforcing the principle of least privilege for network access.

| Component | Role |
| :--- | :--- |
| **AWS Lambda (Python)** | The audit engine. It scans all configured regions, iterates through every Security Group, and flags any rule open to the world (`0.0.0.0/0`). |
| **Amazon EventBridge** | Schedules the Lambda function to run hourly, ensuring continuous monitoring. |
| **AWS IAM Policy** | Grants the necessary permissions (EC2 read-only, S3 write, SNS publish) to the Lambda execution role. |
| **Amazon SNS** | Used to publish an alert message containing a list of non-compliant SGs when findings are detected. |
| **Amazon S3** | Stores a timestamped, detailed JSON report of all findings for historical auditing and compliance purposes. |

***

## Key Benefits

| Benefit | Description |
| :--- | :--- |
| **Automated Auditing** | Eliminates manual checks, providing a consistent and reliable audit of your security groups across all regions every hour. |
| **Enhanced Security Posture** | Proactively identifies and flags potential security risks where resources are unnecessarily exposed to the entire internet. |
| **Immediate Alerting** | Uses SNS to send prompt notifications when a non-compliant security group is discovered, enabling rapid remediation by the operations team. |
| **Compliance & Traceability** | Creates a persistent, auditable trail of security findings in S3, essential for compliance frameworks and historical analysis. |
| **Scalability** | Easily scales to monitor any number of AWS regions and thousands of security groups without requiring changes to the core code. |

***

## Setup and Configuration

1.  **IAM Role:** Create a Lambda Execution Role with the provided IAM Policy (ensuring `YourAMWID` and `YourBucketName` are updated).
2.  **Lambda:** Deploy the `lambda.py` code as an AWS Lambda function, using the created IAM Role.
3.  **SNS:** Create the SNS topic named `SGCompliaceNotification` and subscribe endpoints (e.g., email addresses) to receive alerts.
4.  **S3:** Create the S3 bucket (`YourBucketName`) for storing the audit reports.
5.  **EventBridge:** Configure an EventBridge Rule to invoke the Lambda function on a schedule (e.g., a **Rate of 1 hour**).

![image alt](https://github.com/Tejesh1105/Lambda-Project/blob/369d9ccec6d0ebb0bcb253904cd27182b8290668/Outputs/Lamda%20Function%20Result.PNG)
![image alt](https://github.com/Tejesh1105/Lambda-Project/blob/369d9ccec6d0ebb0bcb253904cd27182b8290668/Outputs/Lamda-S3Bucket-Output-files.PNG)

