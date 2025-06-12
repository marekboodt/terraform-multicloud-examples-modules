# Terraform Infrastructure Repository

This repository contains the Infrastructure as Code (IaC) definitions written in **Terraform**, used to manage and deploy cloud resources securely and consistently.

## 🔒 Security Scanning with Checkov

This repository is integrated with automated security scanning using [Checkov](https://github.com/bridgecrewio/checkov), which runs on every push to the `main` branch. The scans help identify misconfigurations and enforce compliance with security best practices.

### ✅ What Is Checked

- AWS, Azure, and GCP Terraform resources
- Missing encryption
- Public access exposure
- IAM policy over-permissiveness
- And more...

### 🧪 Scan Configuration

A `.checkov.yml` file in the root directory defines **allowed exceptions** for specific findings. These are vetted and justified rule suppressions.

```yaml
# Example: Skip S3 versioning requirement for a non-critical bucket
skip-check:
  - CKV_AWS_23  # Reason: This S3 bucket doesn't need versioning due to data retention policy
