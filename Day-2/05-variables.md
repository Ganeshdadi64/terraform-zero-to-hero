
# Varibles theory
# =============================

```
🧠 1. Theoretical Understanding of Variables
🔑 Why Variables Exist in Terraform?

Terraform follows Infrastructure as Code (IaC) principles.

👉 Without variables:

Code becomes hardcoded
Not reusable
Difficult to manage across environments

👉 With variables:

Same code works for Dev / QA / Prod
Easily configurable
More modular
📥 2. Input Variables (Deep Theory)
🔍 Definition

👉 Input variables are parameters that allow external values to be passed into Terraform configurations.

🧩 Structure of Input Variable
variable "name" {
  description = "what this variable does"
  type        = string
  default     = "value"
}
🔬 Important Concepts
1️⃣ Type System (Very Important)

Terraform supports multiple types:

Type	Example
string	"t2.micro"
number	2
bool	true
list	["us-east-1", "us-west-2"]
map	{ env = "dev" }
object	complex structure
2️⃣ Default Value
Optional
If not provided → Terraform asks user
3️⃣ Validation (Advanced 🔥)
variable "instance_type" {
  type = string

  validation {
    condition     = contains(["t2.micro", "t3.micro"], var.instance_type)
    error_message = "Invalid instance type!"
  }
}

👉 Ensures only valid values are used

🚀 Real Example (Production Style)
📁 variables.tf
variable "env" {
  type    = string
  default = "dev"
}

variable "instance_type" {
  type = string
}

variable "tags" {
  type = map(string)
}
📁 terraform.tfvars
env           = "prod"
instance_type = "t3.medium"

tags = {
  Owner = "Ganesh"
  Env   = "Production"
}
📁 main.tf
resource "aws_instance" "app" {
  ami           = "ami-123456"
  instance_type = var.instance_type

  tags = var.tags
}

👉 Result:

Dev → small instance
Prod → bigger instance
📤 3. Output Variables (Deep Theory)
🔍 Definition

👉 Output variables expose internal Terraform values to:

CLI output
Other modules
External systems
🧩 Structure
output "name" {
  description = "what this output is"
  value       = <expression>
}
🔬 Key Concepts
1️⃣ Value Sources

Output can come from:

Resource attributes
Computed values
Expressions
2️⃣ Sensitive Output 🔥
output "db_password" {
  value     = "mypassword"
  sensitive = true
}

👉 Hidden in CLI output

🔗 4. Input + Output Together (Real Scenario)
🎯 Use Case: Create EC2 and share its IP
📁 main.tf
variable "instance_type" {
  default = "t2.micro"
}

resource "aws_instance" "app" {
  ami           = "ami-123456"
  instance_type = var.instance_type
}
📁 outputs.tf
output "public_ip" {
  value = aws_instance.app.public_ip
}

👉 After apply:

public_ip = 54.23.11.90
📦 5. Module Communication (Very Important 🔥)
🧠 Concept

👉 Modules don’t share data directly
👉 They use inputs & outputs

📁 Module (ec2-module)
resource "aws_instance" "app" {
  ami           = "ami-123"
  instance_type = "t2.micro"
}

output "instance_id" {
  value = aws_instance.app.id
}
📁 Root Module
module "ec2" {
  source = "./ec2-module"
}

output "final_id" {
  value = module.ec2.instance_id
}

👉 Flow:

Module → Output → Root → Use Anywhere
🔄 6. Variable Precedence (VERY IMPORTANT FOR INTERVIEW)

👉 If multiple values are provided, Terraform follows order:

CLI (-var)
.tfvars file
Environment variables
Default value
⚠️ 7. Common Mistakes
❌ Hardcoding values

👉 Not reusable

❌ Not using types

👉 Can cause runtime errors

❌ Exposing sensitive data

👉 Always use sensitive = true

🎯 Final Interview Answer (Strong Version)

👉 “In Terraform, input variables are used to parameterize configurations, enabling reusable and environment-specific deployments, while output variables are used to expose resource attributes and enable communication between modules. Together, they help build modular, scalable, and maintainable infrastructure.”

```





# Variables

Input and output variables in Terraform are essential for parameterizing and sharing values within your Terraform configurations and modules. They allow you to make your configurations more dynamic, reusable, and flexible.

## Input Variables

Input variables are used to parameterize your Terraform configurations. They allow you to pass values into your modules or configurations from the outside. Input variables can be defined within a module or at the root level of your configuration. Here's how you define an input variable:

```hcl
variable "example_var" {
  description = "An example input variable"
  type        = string
  default     = "default_value"
}
```

In this example:

- `variable` is used to declare an input variable named `example_var`.
- `description` provides a human-readable description of the variable.
- `type` specifies the data type of the variable (e.g., `string`, `number`, `list`, `map`, etc.).
- `default` provides a default value for the variable, which is optional.

You can then use the input variable within your module or configuration like this:

```hcl
resource "example_resource" "example" {
  name = var.example_var
  # other resource configurations
}
```

You reference the input variable using `var.example_var`.

## Output Variables

Output variables allow you to expose values from your module or configuration, making them available for use in other parts of your Terraform setup. Here's how you define an output variable:

```hcl
output "example_output" {
  description = "An example output variable"
  value       = resource.example_resource.example.id
}
```

In this example:

- `output` is used to declare an output variable named `example_output`.
- `description` provides a description of the output variable.
- `value` specifies the value that you want to expose as an output variable. This value can be a resource attribute, a computed value, or any other expression.

You can reference output variables in the root module or in other modules by using the syntax `module.module_name.output_name`, where `module_name` is the name of the module containing the output variable.

For example, if you have an output variable named `example_output` in a module called `example_module`, you can access it in the root module like this:

```hcl
output "root_output" {
  value = module.example_module.example_output
}
```

This allows you to share data and values between different parts of your Terraform configuration and create more modular and maintainable infrastructure-as-code setups.
