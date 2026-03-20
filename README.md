# 🚀 Step-by-Step Guide to Setting Up Terraform, AWS CLI, and Your AWS Environment

## ✍️ Introduction  
In this guide, I’ll walk through exactly how I set up:
 - An AWS account (with billing tips)
 - IAM user and MFA authentication
 - AWS CLI on Ubuntu
 - Terraform and connecting it to AWS
   
This is a first-hand guide based on my actual setup experience on Ubuntu.

---

# ☁️ 1. Creating an AWS Account

## Steps
1. Go to https://aws.amazon.com/
2. Click **Create an AWS Account**
3. Enter email, password, and account name
4. Add billing details
5. Verify identity
6. Select **Basic (Free)** plan

## 💡 Billing Tips
- Enable billing alerts
- Create a budget (e.g., $5)
- Stay within free tier



---

# 🔐 2. IAM User & MFA Setup

## Create IAM User
- IAM → Users → Create user
- Username: `terraform-user`
- Enable console + programmatic access

## Permissions
- Attach: `AdministratorAccess`

## Enable MFA
- IAM → User → Security credentials
- Assign MFA (Authenticator app)



---

# 💻 3. Install AWS CLI

```bash
sudo apt update
sudo apt install unzip curl -y

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

## Verify
```bash
aws --version
```

## Configure
```bash
aws configure
```


---

# 🏗️ 4. Install Terraform

```bash
sudo apt update && sudo apt install -y gnupg software-properties-common

wget -O- https://apt.releases.hashicorp.com/gpg | \
 gpg --dearmor | \
 sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg

 echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
 https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
 sudo tee /etc/apt/sources.list.d/hashicorp.list

sudo apt update
sudo apt install terraform
```

## Verify
```bash
terraform -v
```

---

# ⚠️ Issues Encountered

## GPG Warning
```
W: Key is stored in legacy trusted.gpg keyring
```

## Duplicate Repo Warning
```
W: Target Packages is configured multiple times
```

## Fix
```bash
sudo rm /etc/apt/sources.list.d/archive_uri-https_apt_releases_hashicorp_com-noble.list
sudo apt update
```

---

# 🔗 5. Connect Terraform to AWS

Terraform uses AWS CLI credentials automatically.

---

# 🧪 6. First Terraform Project

```bash
mkdir terraform-aws-demo
cd terraform-aws-demo
```

## main.tf
```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "my_bucket" {
  bucket = "my-first-terraform-bucket-12345"
}
```

## Run
```bash
terraform init
terraform plan
terraform apply
```

## Cleanup
```bash
terraform destroy
```

---

# 📊 Architecture

```
Terraform CLI → AWS CLI → AWS
```

---

# 🎯 Key Takeaways
- Use IAM + MFA
- Monitor billing
- Terraform simplifies infrastructure
- Errors are part of learning

---

