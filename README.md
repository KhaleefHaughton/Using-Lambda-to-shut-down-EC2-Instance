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

aws-ec2-auto-shutdown/
├── lambda/
│   ├── shutdown.py          # Python Lambda function to stop EC2 instances
│   └── requirements.txt     # Python dependencies (optional, boto3 included in Lambda)
│
├── .github/
│   └── workflows/
│       └── deploy-lambda.yml # GitHub Actions CI/CD pipeline for Lambda deployment
│
├── .gitignore               # Git ignore rules for Python, AWS, and CI/CD artifacts
└── README.md                # Project documentation and setup guide


aws configure
#Type in your own Id's from above. (Plug in your own)

aws ec2 run-instances --image-id ami-08a6efd148b1f7504 \
--instance-type t2.micro \
--key-name shutdown \
--security-group-ids "sg-0c94988a0d0f178db" \
--count 1

## 2. Configure the Python Lambda 

import boto3  
region = 'us-east-1'
instances = ['i-02490cb03dc73f4e1']
ec2 = boto3.client('ec2', region_name=region)

def lambda_handler(event, context):
    ec2.stop_instances(InstanceIds=instances)
    print('stopped your instances: ' + str(instances))

## 3. Create an EventBridge (CloudWatch) Rule

Inside AWS Console (or via CLI):

Go to EventBridge → Rules → Create rule

Add a schedule like: cron(0 19 * * ? *) (7 PM daily)

Set Target → your Lambda function

This ensures your Lambda runs nightly and stops instances.

## 4. GitHub Actions CI/CD

name: Deploy Lambda

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout
      uses: actions/checkout@v3

    - name: Configure AWS Credentials
      uses: aws-actions/configure-aws-credentials@v2
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us‑east‑1

    - name: Zip Lambda Function
      run: |
        cd lambda
        zip shutdown.zip shutdown.py

    - name: Deploy Lambda
      run: |
        aws lambda update‑function‑code \
          --function‑name auto‑shutdown‑ec2 \
          --zip‑file fileb://lambda/shutdown.zip


    

