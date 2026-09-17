# DynamoDB on LocalStack

A simple local setup for running Amazon DynamoDB with LocalStack and provisioning the table using Terraform.

This project demonstrates how to:

* Run DynamoDB locally with LocalStack
* Create a DynamoDB table using Terraform
* Insert and read data using the AWS CLI
* Work without creating resources in AWS

## Tech Stack

* DynamoDB
* LocalStack
* Terraform
* Docker
* AWS CLI

## Project Structure

```text
.
├── docker-compose.yml
├── provider.tf
├── dynamodb.tf
└── README.md
```

## 1. Start LocalStack

```bash
docker compose up -d
```

Verify that LocalStack is running:

```bash
docker ps
```

## 2. Initialize Terraform

```bash
terraform init
```

## 3. Create the DynamoDB Table

```bash
terraform apply
```

Type `yes` when prompted.

The project creates a table called:

```text
users
```

with `user_id` as the partition key.

## 4. Verify the Table

```bash
aws dynamodb list-tables \
  --endpoint-url http://localhost:4566 \
  --region us-east-1
```

Expected result:

```json
{
  "TableNames": [
    "users"
  ]
}
```

## 5. Add an Item

```bash
aws dynamodb put-item \
  --table-name users \
  --item '{"user_id":{"S":"101"},"name":{"S":"Prerna"}}' \
  --endpoint-url http://localhost:4566 \
  --region us-east-1
```

## 6. Read the Item

```bash
aws dynamodb get-item \
  --table-name users \
  --key '{"user_id":{"S":"101"}}' \
  --endpoint-url http://localhost:4566 \
  --region us-east-1
```

Expected result:

```json
{
  "Item": {
    "user_id": {
      "S": "101"
    },
    "name": {
      "S": "Prerna"
    }
  }
}
```

## Notes

This project uses a pinned LocalStack version so the example can run locally without requiring a LocalStack account.

The Terraform AWS provider is also pinned to the version used and tested with this setup.

## Medium Article

This repository accompanies my Medium article:

**DynamoDB on LocalStack: A 10-Minute Setup Guide**

*Add the Medium article link here after publishing.*
