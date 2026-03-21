# aws-python-playbook-
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
