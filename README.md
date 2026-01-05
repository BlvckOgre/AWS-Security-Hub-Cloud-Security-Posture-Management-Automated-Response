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


