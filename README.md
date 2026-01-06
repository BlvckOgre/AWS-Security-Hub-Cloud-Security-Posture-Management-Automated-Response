# AWS Security Hub: Cloud Security Posture Management & Automated Response

> A practical, infrastructure-as-code driven implementation of AWS Security Hub with automated detection, correlation, and remediation using EventBridge and Lambda.

This project demonstrates how to deploy, operate, and extend **AWS Security Hub** as a **Cloud Security Posture Management (CSPM)** solution using **Terraform and CloudFormation**, with **event-driven security automation** for real-world cloud environments.

The focus is not just enabling the service, but **operating it effectively**: reducing noise, prioritising risk, and responding automatically to high-confidence security findings.

---

## 📌 Project Objectives

By completing this project, you will be able to:

- Understand how **AWS Security Hub** fits into a cloud security architecture
- Enable and manage **security standards** (AWS Best Practices, CIS)
- Aggregate findings across AWS security services
- Deploy Security Hub using **Infrastructure as Code**
- Route findings via **Amazon EventBridge**
- Automatically remediate issues using **AWS Lambda**
- Apply CSPM operational best practices used in enterprise environments

---

## 🧠 Key Concepts Covered

- Cloud Security Posture Management (CSPM)
- AWS-native security tooling
- Security standards & benchmarks
- Event-driven security automation
- Infrastructure as Code (IaC)
- Detection → Response workflows
- Security operations at scale

---

## 🖼️ High-Level Architecture

<!-- IMAGE PLACEHOLDER -->
<!-- Diagram: AWS Services → Security Hub → EventBridge → Lambda → Optional SIEM/SNS -->
![AWS Security Hub Architecture](images/security-hub-architecture.png)

---

## 🔐 What is AWS Security Hub?

**AWS Security Hub** is a native AWS service that provides a **centralised view of your security posture** across one or more AWS accounts.

It:
- Collects findings from AWS security services (GuardDuty, Inspector, Macie)
- Correlates and prioritises findings
- Continuously evaluates resources against industry standards
- Acts as a control plane for cloud security monitoring and response

Because it is AWS-native, Security Hub avoids the complexity of third-party identity federation and can be enabled immediately.

---

## ✨ Core Features

### 1️⃣ Cloud Security Posture Reports

Security Hub continuously evaluates your environment against:

- AWS Foundational Security Best Practices
- CIS AWS Foundations Benchmark
- PCI DSS (where applicable)

These reports:
- Highlight misconfigurations
- Track compliance over time
- Provide measurable security posture metrics

<!-- IMAGE PLACEHOLDER -->
![Posture Reports](images/security-posture.png)

---

### 2️⃣ Correlated Security Findings

Security Hub aggregates findings from:
- Amazon GuardDuty
- AWS Inspector
- Amazon Macie
- Third-party security products

Correlated findings provide a **prioritised risk view**, enabling faster incident response.

<!-- IMAGE PLACEHOLDER -->
![Correlated Findings](images/correlated-findings.png)

---

### 3️⃣ Security Automation & Response

Using **EventBridge**, Security Hub findings can trigger:
- AWS Lambda functions
- Step Functions workflows
- Notifications to SIEMs or ticketing systems

This project focuses on **safe, automated remediation** for high-confidence findings.

<!-- IMAGE PLACEHOLDER -->
![Automation Flow](images/security-automation.png)

---

## 🛠️ Deployment Options

This project supports **both Terraform and CloudFormation**, reflecting real-world environments.

---

## 🧱 Terraform Implementation

### 📁 Directory Structure

terraform/
├── main.tf
├── securityhub.tf
├── eventbridge.tf
├── lambda.tf
├── iam.tf
└── variables.tf

lambda/
└── auto_remediate_s3_public_access.py


---

### 🔐 Enable AWS Security Hub

```hcl
resource "aws_securityhub_account" "this" {}

📊 Enable AWS Foundational Security Best Practices
resource "aws_securityhub_standards_subscription" "aws_best_practices" {
  standards_arn = "arn:aws:securityhub:::standards/aws-foundational-security-best-practices/v/1.0.0"
}

### 🚨 EventBridge Rule for Critical Findings
resource "aws_cloudwatch_event_rule" "securityhub_critical" {
  name = "securityhub-critical-findings"

  event_pattern = jsonencode({
    source      = ["aws.securityhub"],
    detail-type = ["Security Hub Findings - Imported"],
    detail = {
      findings = {
        Severity = {
          Label = ["CRITICAL"]
        }
      }
    }
  })
}

### 🎯 EventBridge → Lambda Target
resource "aws_cloudwatch_event_target" "lambda_target" {
  rule = aws_cloudwatch_event_rule.securityhub_critical.name
  arn  = aws_lambda_function.auto_remediate.arn
}

### 🤖 Lambda Auto-Remediation Example
Use Case

Automatically block public access on S3 buckets when flagged by Security Hub.

Lambda Function (Python)
import json
import boto3

s3 = boto3.client("s3")

def lambda_handler(event, context):
    findings = event["detail"]["findings"]

    for finding in findings:
        for resource in finding.get("Resources", []):
            if resource["Type"] == "AwsS3Bucket":
                bucket = resource["Id"].split(":::")[-1]

                s3.put_public_access_block(
                    Bucket=bucket,
                    PublicAccessBlockConfiguration={
                        "BlockPublicAcls": True,
                        "IgnorePublicAcls": True,
                        "BlockPublicPolicy": True,
                        "RestrictPublicBuckets": True
                    }
                )

    return {"status": "remediation complete"}

☁️ CloudFormation Alternative
AWSTemplateFormatVersion: "2010-09-09"

Resources:
  SecurityHub:
    Type: AWS::SecurityHub::Hub

  SecurityStandard:
    Type: AWS::SecurityHub::StandardsSubscription
    Properties:
      StandardsArn: arn:aws:securityhub:::standards/aws-foundational-security-best-practices/v/1.0.0

### ⚠️ Operational Challenges (The Real Work)

Turning on Security Hub is easy. Operating it well is hard.

Key challenges include:

Reducing noisy findings

Aligning alerts to real risk

Designing centralised security accounts

Routing findings into operational workflows

Automating remediation safely

Security Hub delivers value only when paired with process, automation, and ownership.

### 🏗️ Best Practices Applied

Deploy via Infrastructure as Code

Centralise findings across accounts

Focus automation on high-confidence issues

Keep humans in the loop for destructive actions

Continuously tune controls and severity thresholds

### 🧯 Disable After Testing

To avoid unexpected costs:

Security Hub → Settings → General → Disable


Always disable unused services in lab environments.


