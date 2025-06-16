# Terraform Infrastructure Multicloud Examples Modules Repository

This repository contains the **Infrastructure as Code (IaC)** definitions written in **Terraform**, used to manage and deploy cloud resources securely and consistently across environments.

---

## 🔒 Security Scanning with Checkov

This repository is integrated with automated security scanning using [Checkov](https://www.checkov.io), executed via GitHub Actions every time code is pushed to the `main` branch.

The purpose is to catch and prevent misconfigurations in Terraform code that could lead to insecure infrastructure deployments.

---

## ✅ What Is Checked?

Checkov analyzes Terraform code and reports potential risks such as:

- AWS, Azure, and GCP resource misconfigurations
- Unencrypted resources
- Publicly exposed services (e.g., open S3 buckets or public VMs)
- IAM policies with wildcard access
- Missing logging or monitoring settings
- Weak security group rules (e.g., open ports or 0.0.0.0/0)

---

## 📁 Supported Project Structures

The pipeline supports flexible layouts and automatically scans valid Terraform code in:

### ✅ Flat Structure
For smaller, monolithic projects:

main.tf
variables.tf
outputs.tf

### ✅ Subfolder Structure
For multi-cloud or environment-separated stacks:

aws/  
└── main.tf  
azure/  
└── main.tf  
gcp/  
└── main.tf  

These subdirectories are detected and scanned automatically. You do **not** need to move your code into a folder — both styles work as long as `project_dir: ./` is passed in your workflow configuration.

---

## 🗂️ Shell Script Output (multi-directory scans)
### Folder:  
Terraform-ALL-IAC-SCANS-{environment}-{timestamp}

### Files:  
Terraform-IAC-CHECKOV-{folder}-{environment}-{timestamp}.txt

## 🗂️ GitHub Action Output (combined scan)
### Folder:  
terraform-ALL-IAC-GITHUB-ACTION-SCANS-{environment}-{timestamp}

### Files:  
terraform-IAC-Checkov-githubactions-scan-{environment}-{timestamp}.json  
terraform-IAC-Checkov-githubactions-scan-{environment}-{timestamp}.txt  
terraform-IAC-Checkov-githubactions-scan-{environment}-{timestamp}.sarif  

## ⚙️ .github/workflows File: `main.yml`

You can include an optional `.checkov.yml` file at the root of your repository to suppress specific rules globally, with clear justifications.

### Example:

```yaml
# Testing to "accept" risks / vulnerabilities
name: Terraform  # workflow name
on:
  push:
    branches:
      - main

permissions:
  actions: read
  contents: read

jobs:
  IAC-Workflow:
    uses: marekboodt/Security-Scanning-Repo/.github/workflows/iac-workflow.yml@main
    with:
      language: terraform
      project_dir: ./
      # non-prod has 'continue-on-error: true' all other environments will be stopped if the scans give an error.
      environment: non-prod
```
## ⚙️ Configuration File: `.checkov.yml`

You can include an optional `.checkov.yml` file at the root of your repository to suppress specific rules globally, with clear justifications.

### Example:

```yaml
# Testing to "accept" risks / vulnerabilities
skip-check:
  - CKV_AWS_23  # Reason: This S3 bucket doesn't need versioning due to data retention policy
