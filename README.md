# Terraform Module: S3 to SQS Claim Check Pattern

This Terraform module provisions the necessary AWS infrastructure to implement the [Claim Check pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/claim-check), where large or sensitive payloads are stored in Amazon S3 and referenced via lightweight messages in Amazon SQS.

It supports multiple environments (e.g., `dev`, `staging`, `prod`), enforces encryption at rest and in transit, and integrates easily with existing application IAM roles.



## 📦 Features

- Encrypted S3 bucket for storing large payloads (SSE-KMS)
- Event notification from S3 to SQS
- Encrypted SQS queue for claim-check messages
- Optional KMS key management (create or reuse)
- Environment-specific naming and tagging
- Secure IAM policies to integrate with existing identities



## 🧪 Testing

cd test/
go test -v



## 📁 Module Structure

```text
module_terraform_streamProcess_S3toSQS/
├── modules/
│   └── claim_check/
│       ├── main.tf
│       ├── variables.tf
│       ├── outputs.tf
│       └── README.md
├── environments/
│   ├── dev/
│   └── prod/
├── examples/
│   └── basic/
├── test/
│   └── test_claim_check.go
├── doc/
│   └── architecture.md
└── README.md
```



## 🧩 Inputs

| Name           | Description                                 | Type   | Required |
|----------------|---------------------------------------------|--------|----------|
| environment    | Deployment environment (dev, prod, etc.)    | string | ✅        |
| region         | AWS region to deploy                        | string | ✅        |
| s3_bucket_name | Name of the S3 bucket                       | string | ✅        |
| sqs_queue_name | Name of the SQS queue                       | string | ✅        |
| kms_key_alias  | KMS key alias (optional if using existing)  | string | ✅        |
| tags           | Common tags to apply to resources           | map    | ✅        |



## ✅ Outputs

| Name           | Description                      |
|----------------|----------------------------------|
| s3_bucket_arn  | ARN of the created S3 bucket     |
| sqs_queue_arn  | ARN of the created SQS queue     |
| kms_key_arn    | ARN of the KMS encryption key    |
| queue_url      | URL of the SQS queue             |



## 🚀 Usage

```hcl
module "claim_check" {
  source            = "git::https://github.com/kaimmej/module_terraform_streamProcess_S3toSQS.git"
  
  environment       = "dev"
  region            = "us-west-2"
  s3_bucket_name    = "dev-claimcheck-bucket"
  sqs_queue_name    = "dev-claimcheck-queue"
  kms_key_alias     = "alias/claimcheck-key"
  tags = {
    Project     = "stream-processing"
    Environment = "dev"
  }
}