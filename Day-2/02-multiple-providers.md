# Multiple Providers

You can use multiple providers in one single terraform project. For example,


1. Create a providers.tf file in the root directory of your Terraform project.
2. In the providers.tf file, define the AWS and Azure providers. For example:


```
provider "aws" {
  region = "us-east-1"
}

provider "azurerm" {
  subscription_id = "your-azure-subscription-id"
  client_id = "your-azure-client-id"
  client_secret = "your-azure-client-secret"
  tenant_id = "your-azure-tenant-id"
}
```

3. In your other Terraform configuration files, you can then use the aws and azurerm providers to create resources in AWS and Azure, respectively,

```
resource "aws_instance" "example" {
  ami = "ami-0123456789abcdef0"
  instance_type = "t2.micro"
}

resource "azurerm_virtual_machine" "example" {
  name = "example-vm"
  location = "eastus"
  size = "Standard_A1"
}
```



# Best example

```
🌍 What is Multiple Providers in Terraform?

👉 It means using different cloud platforms in the same Terraform project

For example:

AWS → EC2
Azure → Virtual Machine

All managed in one codebase

🧠 Real-Life Scenario

Imagine your company:

Uses Amazon Web Services for backend services
Uses Microsoft Azure for analytics or internal tools

👉 Instead of writing separate Terraform projects, you manage both together.

🔑 Key Idea

Terraform supports multiple providers like:

AWS (aws)
Azure (azurerm)
GCP (google)

Each provider = one cloud platform

✅ Step-by-Step Explanation
1️⃣ Define Providers (providers.tf)
provider "aws" {
  region = "us-east-1"
}

provider "azurerm" {
  features {}

  subscription_id = "your-subscription-id"
  client_id       = "your-client-id"
  client_secret   = "your-secret"
  tenant_id       = "your-tenant-id"
}

👉 What’s happening:

AWS provider → connects to AWS
Azure provider → connects to Azure
2️⃣ Create Resources from Different Clouds
🟢 AWS Resource (EC2)
resource "aws_instance" "example" {
  ami           = "ami-0123456789abcdef0"
  instance_type = "t2.micro"
}
🔵 Azure Resource (VM)
resource "azurerm_virtual_machine" "example" {
  name     = "example-vm"
  location = "eastus"
  vm_size  = "Standard_B1s"
}
⚡ What Terraform Does

👉 During terraform apply:

Connects to AWS → creates EC2
Connects to Azure → creates VM

All in one execution

🔄 Visual Understanding
Terraform
   |
   |---- AWS Provider → EC2 Instance
   |
   |---- Azure Provider → Virtual Machine
🚀 Real DevOps Use Cases
1. Multi-Cloud Strategy

Avoid vendor lock-in
Use AWS + Azure together

2. Migration Projects

Move apps from AWS → Azure gradually

3. Best Service Usage
AWS → better compute
Azure → better integration with Microsoft tools
⚠️ Important Points
🔸 Azure Needs features {}

Mandatory block:

features {}
🔸 Authentication is Different
AWS → Access Key / IAM Role
Azure → Service Principal
🔸 Resource Names Are Different
AWS → aws_instance
Azure → azurerm_virtual_machine
🎯 Interview One-Liner

👉 “Terraform supports multiple providers, allowing us to provision and manage infrastructure across different cloud platforms like AWS and Azure within a single project.”


```
