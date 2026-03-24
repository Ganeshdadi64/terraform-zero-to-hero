# Multiple Region Implementation in Terraform

You can make use of `alias` keyword to implement multi region infrastructure setup in
terraform.

```
provider "aws" {
  alias = "us-east-1"
  region = "us-east-1"
}

provider "aws" {
  alias = "us-west-2"
  region = "us-west-2"
}

resource "aws_instance" "example" {
  ami = "ami-0123456789abcdef0"
  instance_type = "t2.micro"
  provider = "aws.us-east-1"
}

resource "aws_instance" "example2" {
  ami = "ami-0123456789abcdef0"
  instance_type = "t2.micro"
  provider = "aws.us-west-2"
}
```

# Best Example

```

🌍 What is Multi-Region in Terraform?

Normally, Terraform uses one AWS region (like us-east-1).

But in real projects, you may need:

Disaster Recovery (DR)
High Availability
Global applications

👉 So you deploy resources in multiple regions.

🔑 Key Concept: alias in Provider

Terraform allows multiple AWS configurations using alias.

Think like this:

One AWS account
Multiple regions
Each region = separate provider config
🧠 Simple Real-Life Example

Imagine you are building an application:

Primary server → us-east-1
Backup server → us-west-2
✅ Step-by-Step Terraform Example
1️⃣ Define Multiple Providers
provider "aws" {
  alias  = "east"
  region = "us-east-1"
}

provider "aws" {
  alias  = "west"
  region = "us-west-2"
}

👉 Here:

east = us-east-1
west = us-west-2
2️⃣ Create Resources in Different Regions
🟢 EC2 in US-East (Primary)
resource "aws_instance" "app_server_primary" {
  ami           = "ami-0abcdef1234567890"
  instance_type = "t2.micro"

  provider = aws.east
}
🔵 EC2 in US-West (Backup)
resource "aws_instance" "app_server_backup" {
  ami           = "ami-0abcdef1234567890"
  instance_type = "t2.micro"

  provider = aws.west
}
⚡ What’s Happening Here?
Resource	Region	Provider Used
Primary EC2	us-east-1	aws.east
Backup EC2	us-west-2	aws.west

👉 Terraform knows where to create resources based on:

provider = aws.<alias>
🚀 Why This is Useful (Real DevOps Use Cases)
1. Disaster Recovery

If us-east-1 fails → traffic goes to us-west-2

2. High Availability

Run apps in multiple regions

3. Latency Optimization

Users connect to nearest region

⚠️ Common Mistake

❌ Wrong:

provider = "aws.us-east-1"

✅ Correct:

provider = aws.east

👉 Always use:

aws.<alias>
🎯 Interview One-Liner

👉 “In Terraform, multi-region infrastructure is implemented using multiple provider blocks with aliases, and resources are mapped to specific regions using the provider argument.”

💡 Pro Tip (Advanced)

You can also:

Create modules per region
Use variables to dynamically select regions
Combine with Route 53 for global routing


```
