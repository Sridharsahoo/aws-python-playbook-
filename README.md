# 🚀 AWS Python Playbook for DevOps & Cloud Architects

A practical reference guide to use **Python (boto3) with AWS services** like DynamoDB, SNS, Lambda, S3, and EC2.

👉 Designed for:

* DevOps Engineers
* Cloud Architects
* SREs
* Platform Engineers

---

# 📌 🧠 Core Concept (Most Important)

Every AWS interaction in Python follows this pattern:

```python
import boto3

# Low-level client
client = boto3.client('service-name')

# OR high-level resource
resource = boto3.resource('service-name')
```

---

# 🥇 DynamoDB Playbook

## 🔹 Import

```python
from boto3.dynamodb.conditions import Key, Attr
```

## 🔹 Create Resource

```python
import boto3
dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('MyTable')
```

## 🔹 Query (Primary Key)

```python
response = table.query(
    KeyConditionExpression=Key('CustomerId').eq('1')
)

items = response['Items']
```

👉 Equivalent to:

```
SELECT * WHERE CustomerId = 1
```

## 🔹 Filter (Non-Key)

```python
FilterExpression=Attr('OrderState').eq('APPROVED')
```

---

# 🥈 SNS Playbook

## 🔹 Create Client

```python
sns = boto3.client('sns')
```

## 🔹 Publish Message

```python
sns.publish(
    TopicArn='your-topic-arn',
    Message='Hello World'
)
```

## 🔹 Send JSON

```python
import json

sns.publish(
    TopicArn='your-topic-arn',
    Message=json.dumps({'status': 'success'})
)
```

---

# 🥉 Lambda Playbook

## 🔹 Basic Structure

```python
def lambda_handler(event, context):
    print(event)
```

## 🔹 Production Pattern

```python
def lambda_handler(event, context):
    try:
        # your logic
        return {"status": "success"}
    except Exception as e:
        print(e)
        return {"status": "error"}
```

---

# 🏅 X-Ray (Monitoring)

## 🔹 Enable Tracing

```python
from aws_xray_sdk.core import patch_all
patch_all()
```

## 🔹 Trace Function

```python
from aws_xray_sdk.core import xray_recorder

@xray_recorder.capture("my_function")
def my_function():
    pass
```

---

# 🧱 S3 Playbook

## 🔹 Upload File

```python
s3 = boto3.client('s3')

s3.upload_file('file.txt', 'bucket-name', 'file.txt')
```

## 🔹 Download File

```python
s3.download_file('bucket-name', 'file.txt', 'local.txt')
```

---

# ⚡ EC2 Playbook

## 🔹 Start Instance

```python
ec2 = boto3.client('ec2')

ec2.start_instances(InstanceIds=['i-123456'])
```

## 🔹 Stop Instance

```python
ec2.stop_instances(InstanceIds=['i-123456'])
```

---

# 🔥 Error Handling Pattern

```python
try:
    response = table.query(...)
except Exception as e:
    print("Error:", e)
```

---
# 🔗 API Gateway Playbook

## 🔹 Use Case

Expose Lambda as an HTTP API

## 🔹 Lambda Example (API Response)

```python
def lambda_handler(event, context):
    return {
        "statusCode": 200,
        "body": "Hello from Lambda"
    }
```

## 🔹 JSON Response

```python
import json

def lambda_handler(event, context):
    return {
        "statusCode": 200,
        "body": json.dumps({"message": "success"})
    }
```

👉 API Gateway expects:

* `statusCode`
* `body` (string)

---

# 🔁 Step Functions Playbook

## 🔹 Use Case

Orchestrate workflows (multiple Lambdas)

## 🔹 Example Flow

```text
Step1 → Step2 → Step3
```

## 🔹 Basic Lambda Task Input

```json
{
  "orderId": "123"
}
```

## 🔹 Lambda Example

```python
def lambda_handler(event, context):
    order_id = event['orderId']
    return {"status": "processed", "orderId": order_id}
```

## 🔹 Key Concept

* State Machine controls execution
* Lambda performs tasks

---

# ⏰ EventBridge Playbook

## 🔹 Use Case

Trigger Lambda on schedule or events

## 🔹 Schedule Example (Cron)

```text
cron(0 13 * * ? *)  # 7 PM IST
```

## 🔹 Lambda Example

```python
def lambda_handler(event, context):
    print("Triggered by EventBridge")
```

## 🔹 Common Use Cases

* EC2 start/stop automation
* Daily batch jobs
* Monitoring triggers

---

# 📬 SQS Playbook

## 🔹 Use Case

Queue-based decoupling (async processing)

## 🔹 Send Message

```python
import boto3

sqs = boto3.client('sqs')

sqs.send_message(
    QueueUrl='your-queue-url',
    MessageBody='Hello from SQS'
)
```

## 🔹 Receive Message

```python
response = sqs.receive_message(
    QueueUrl='your-queue-url',
    MaxNumberOfMessages=1
)

messages = response.get('Messages', [])
```

## 🔹 Lambda Trigger (Best Practice)

* SQS → Lambda (automatic trigger)
* No polling needed

---

# 🧠 Architecture Patterns

## 🔹 Event-Driven Pattern

```text
EventBridge → Lambda → SNS/SQS
```

## 🔹 Decoupled Architecture

```text
Producer → SQS → Lambda → DB
```

## 🔹 API-Based Architecture

```text
API Gateway → Lambda → DynamoDB
```

## 🔹 Workflow Orchestration

```text
Step Functions → Lambda → Multiple Services
```

---

# 🔥 Real-World Example (Your Use Case)

## EC2 Cost Optimization

```text
EventBridge (schedule)
        ↓
Lambda (Python)
        ↓
EC2 start/stop
```

---

# 🧠 When to Use What

| Service        | Use Case               |
| -------------- | ---------------------- |
| API Gateway    | Expose APIs            |
| Lambda         | Compute logic          |
| DynamoDB       | NoSQL storage          |
| SNS            | Notifications          |
| SQS            | Queue / buffering      |
| EventBridge    | Scheduling/events      |
| Step Functions | Workflow orchestration |

---

# 🚀 Architect Tips

## ✅ Use SQS when:

* Need buffering
* Handle traffic spikes

## ✅ Use SNS when:

* Fan-out notifications
* Multiple subscribers

## ✅ Use Step Functions when:

* Multi-step workflows
* Error handling between steps

## ✅ Use EventBridge when:

* Scheduling
* Event-driven automation

---

# ⚡ Production Best Practices

* Use DLQ (Dead Letter Queue) for SQS/Lambda
* Enable retries with backoff
* Use IAM least privilege
* Use environment variables
* Add CloudWatch logging & alarms

---

# 🧠 Final Insight

> Don’t think in services.
> Think in **patterns**:

* Event-driven
* Async processing
* Microservices orchestration

---

⭐ Keep extending this playbook as you build real projects!

# 🌍 Environment Variables (Best Practice)

```python
import os

table_name = os.environ['TABLE_NAME']
```

👉 Avoid hardcoding values

---

# 🧩 Combined Example

```python
import boto3
import json

sns = boto3.client('sns')

def lambda_handler(event, context):
    try:
        message = {"status": "ok"}

        sns.publish(
            TopicArn='arn:aws:sns:region:account-id:topic',
            Message=json.dumps(message)
        )

        return {"status": "success"}

    except Exception as e:
        print(e)
        return {"status": "error"}
```

---

# 🧠 How to Master This (Important)

## ✅ 1. Learn Patterns (Not Syntax)

* DynamoDB → `Key`, `Attr`
* SNS → `publish`
* EC2 → `start_instances`

## ✅ 2. Practice Core Use Cases

* Lambda → DynamoDB
* Lambda → SNS
* Lambda → S3
* Lambda → EC2

## ✅ 3. Use Docs Smartly

Search like:

```
boto3 dynamodb query example
boto3 sns publish example
```

## ✅ 4. Build Your Own Templates

Create reusable snippets for:

* API calls
* Error handling
* Logging

---

# 🧠 Architect Insight

> Coding is NOT about memorizing syntax.
> It’s about understanding patterns and knowing where to find the right solution.

---

# 📚 Recommended References

* AWS boto3 Documentation
* AWS Lambda Developer Guide
* AWS Well-Architected Framework

---

# 🚀 Next Steps

* Build real use cases (Lambda + EventBridge + EC2 automation)
* Integrate with Terraform
* Add CI/CD pipeline (GitHub Actions / GitLab CI)

---

⭐ If this helped you, consider adding improvements and making it your personal playbook!





# 🐳 Docker Integration with AWS Lambda

## 🔹 Overview

AWS Lambda supports container images, allowing you to package your function and dependencies using Docker.

---

## 🔹 Architecture

```text
Developer → Docker Build → ECR → Lambda
```

---

## 🔹 Why Use Docker with Lambda?

* Large dependencies (ML, pandas, numpy)
* Custom runtime support
* Better control over environment
* Consistent deployment across environments

---

## 🔹 Lambda Function Example

```python
# app.py
def lambda_handler(event, context):
    return {
        "statusCode": 200,
        "body": "Hello from Docker Lambda"
    }
```

---

## 🔹 Dockerfile Example

```dockerfile
FROM public.ecr.aws/lambda/python:3.9

COPY app.py ${LAMBDA_TASK_ROOT}

CMD ["app.lambda_handler"]
```

---

## 🔹 Build & Push Steps

```bash
docker build -t my-lambda .

docker tag my-lambda:latest <account-id>.dkr.ecr.<region>.amazonaws.com/my-lambda

docker push <ecr-repo-url>
```

---

## 🔹 Terraform Example

```hcl
resource "aws_lambda_function" "docker_lambda" {
  function_name = "docker-lambda"

  package_type = "Image"
  image_uri    = "<ecr-repo-url>:latest"

  role = aws_iam_role.lambda_role.arn
}
```

---

## 🔹 Best Practices

* Keep image size small
* Use multi-stage builds
* Avoid unnecessary libraries
* Use environment variables for configs

---

## 🔹 When to Use

| Use Case           | Recommendation |
| ------------------ | -------------- |
| Simple Lambda      | ZIP            |
| Heavy dependencies | Docker         |
| ML workloads       | Docker         |
| Custom runtime     | Docker         |

---

## 🔹 Limitations

* Larger cold start time
* Requires Docker knowledge
* Image size optimization needed

---

## 🔹 Summary

Docker-based Lambda provides flexibility and scalability but should be used based on workload complexity and performance requirements.

