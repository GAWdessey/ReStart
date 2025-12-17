# Python Scripting & Cloud Automation

**Project Focus:** Automating AWS tasks, Data Parsing, and Logic Control  
**Tech Stack:** Python 3, Boto3 (AWS SDK), JSON, Linux Environment

## Project Overview
In this module, I transitioned from managing AWS resources manually via the Management Console to programmatically controlling them using **Python**. The focus was on writing clean, efficient scripts to automate repetitive operational tasks and interact with AWS services using the **Boto3 library**.

## Key Concepts Demonstrated
* **Data Structures:** Using Lists and Dictionaries to organize cloud resource data.
* **Control Flow:** Implementing `if/else` logic and `for` loops to iterate through large datasets (e.g., filtering specific EC2 instances).
* **File Handling:** Parsing JSON files to extract configuration details.
* **AWS SDK (Boto3):** Writing scripts to create, list, and manage S3 buckets and EC2 instances directly from the terminal.

---

## Sample Scripts

### 1. AWS S3 Bucket Generator (Boto3)
This script automates the creation of S3 buckets with unique names, handling errors if a bucket name already exists.

```python
import boto3
import random

def create_bucket(bucket_prefix):
    s3 = boto3.client('s3')
    # Generate a random suffix to ensure uniqueness
    bucket_name = f"{bucket_prefix}-{random.randint(1000, 9999)}"
    
    try:
        response = s3.create_bucket(Bucket=bucket_name)
        print(f"Success! Created bucket: {bucket_name}")
        return bucket_name
    except Exception as e:
        print(f"Error creating bucket: {e}")

# Main execution
if __name__ == "__main__":
    create_bucket("my-portfolio-bucket")
2. System Info Extractor (JSON Parsing)
In cloud environments, configuration data is often passed as JSON. This script reads a raw JSON file and extracts specific user data.

Python

import json

# Simulating a JSON payload from an AWS event
json_data = """
{
    "Records": [
        {
            "s3": {
                "bucket": {
                    "name": "data-source-bucket",
                    "arn": "arn:aws:s3:::data-source-bucket"
                },
                "object": {
                    "key": "user_data.csv",
                    "size": 1024
                }
            }
        }
    ]
}
"""

def extract_info(data):
    parsed = json.loads(data)
    bucket_name = parsed['Records'][0]['s3']['bucket']['name']
    file_name = parsed['Records'][0]['s3']['object']['key']
    print(f"Processing File: {file_name} from Bucket: {bucket_name}")

extract_info(json_data)
Reflection
Learning Python has shifted my perspective from "clicking buttons" to "defining logic." The ability to use Boto3 is particularly powerful; it allows me to build infrastructure that is reproducible and scalable. I can now write a single script to deploy resources that would take 20 minutes to configure manually.
