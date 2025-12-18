# Using-Lambda-to-shut-down-EC2-Instance
Shut don’t go up! It Goes Down.

# AWS EC2 Auto Shutdown

This project implements a **serverless AWS Lambda automation** to **automatically shut down non‑production EC2 instances** (e.g., development or test environments) on a schedule — helping reduce cloud costs.

The automation is fully **CI/CD enabled** with **GitHub Actions**, ensuring that infrastructure changes are deployed reliably and consistently.

This project was built based on the workflow described in the Medium article *Shut don’t go up! It Goes Down.* by Khaleef Haughton. :contentReference[oaicite:1]{index=1}

---

## 🧠 What It Does

✔️ Uses AWS Lambda (Python) to stop one or more EC2 instances  
✔️ Lambda is scheduled with AWS EventBridge (formerly CloudWatch Events)  
✔️ Integrates with GitHub Actions to automate deployment  
✔️ Designed for development/test environments to avoid unnecessary costs

---

## 📌 Architecture

1. GitHub Actions triggered on merge to main  
2. Lambda code packaged & deployed using AWS CLI  
3. Lambda runs on a **fixed schedule** (e.g., every night at 7 PM) and stops the defined EC2 instance(s)

---

## 🧱 Prerequisites

Before beginning:

- AWS Account with permissions for IAM, EC2, Lambda, and EventBridge  
- AWS CLI installed and configured (`aws configure`)  
- GitHub repository with repository secrets set

---

## 🛠️ Project Structure

