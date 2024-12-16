---
title: Terraform
date: 2024-10-08T05:43:58.5858+05:30
draft: true
tags:
  - devops
---


developed by hashicrop

HCL language used  for this


Automate and run a platform

Ansible-> configure infra tools 

list of IAAC
- terraform
- aws cloud formation
- azure resource manager
- google cloud deployment manger
- ansible,chef,puppet,saltstack

## HCL

Certainly! HashiCorp Configuration Language (HCL) is a configuration language designed for defining infrastructure as code. It aims to be both human-readable and machine-friendly, making it suitable for managing cloud resources and infrastructure.

### Core Concepts of HCL

#### 1. **Declarative Syntax**
HCL is declarative, meaning we describe the desired state of our infrastructure rather than specifying the steps to achieve that state. For example, we define what resources we want rather than how to create them.

#### 2. **Blocks and Attributes**
HCL configurations are organized into blocks. Each block defines a specific entity (like a resource, provider, or module). Here's a breakdown:

- **Resource Block**: Defines a piece of infrastructure, such as an AWS EC2 instance.
  
  ```hcl
  resource "aws_instance" "example" {
    ami           = "ami-123456"
    instance_type = "t2.micro"
  }
  ```

- **Provider Block**: Specifies the provider (like AWS, Azure, etc.) and its configuration.

  ```hcl
  provider "aws" {
    region = "us-west-2"
  }
  ```

- **Module Block**: Used to encapsulate a group of resources and configurations.

  ```hcl
  module "my_module" {
    source = "./module-directory"
  }
  ```

#### 3. **Variables**
Variables in HCL allow we to parameterize our configurations, making them reusable and adaptable.

- **Variable Declaration**:
  
  ```hcl
  variable "instance_type" {
    description = "Type of EC2 instance"
    default     = "t2.micro"
  }
  ```

- **Using Variables**: we can reference variables using interpolation.

  ```hcl
  resource "aws_instance" "example" {
    ami           = "ami-123456"
    instance_type = var.instance_type
  }
  ```

#### 4. **Outputs**
Outputs provide a way to extract information from our resources after they are created, which can be useful for chaining configurations or displaying important data.

```hcl
output "instance_ip" {
  value = aws_instance.example.public_ip
}
```

#### 5. **Interpolation**
Interpolation allows we to dynamically reference variables and resource attributes. we use `${}` to perform interpolation.

```hcl
resource "aws_instance" "example" {
  ami           = "ami-123456"
  instance_type = "${var.instance_type}"
}
```

#### 6. **Data Sources**
Data sources allow we to fetch data from existing resources. This is useful for using information that is not managed by our configuration.

```hcl
data "aws_ami" "latest" {
  most_recent = true

  owners = ["amazon"]
}
```

#### 7. **Comments**
we can add comments in HCL using `#` for single-line comments or `/* ... */` for multi-line comments.

```hcl
# This is a single-line comment

/*
This is a 
multi-line comment
*/
```

### Example Configuration
Here’s a more comprehensive example that includes providers, resources, variables, outputs, and modules:

```hcl
provider "aws" {
  region = "us-west-2"
}

variable "instance_type" {
  description = "EC2 instance type"
  default     = "t2.micro"
}

resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_subnet" "main" {
  vpc_id     = aws_vpc.main.id
  cidr_block = "10.0.1.0/24"
}

resource "aws_instance" "example" {
  ami           = "ami-123456"
  instance_type = var.instance_type
  subnet_id     = aws_subnet.main.id
}

output "instance_ip" {
  value = aws_instance.example.public_ip
}
```



## Basic Commands

- **Initialize a new or existing Terraform configuration**:  
```bash
  terraform init
```

- **Validate the configuration files**:  
  ```bash
  terraform validate
  ```

- **Format the configuration files**:  
  ```bash
  terraform fmt
  ```

- **Show the execution plan**:  
```bash
  terraform plan
```

- **Apply the changes required to reach the desired state**:  
  ```bash
  terraform apply
  ```

- **Destroy the infrastructure**:  
  ```bash
  terraform destroy
  ```

### Configuration Basics
- **Provider**: Specifies the provider (e.g., AWS, Azure, GCP).  
  ```hcl
  provider "aws" {
    region = "us-west-2"
  }
  ```

- **Resource**: Defines a single resource.  
  ```hcl
  resource "aws_instance" "example" {
    ami           = "ami-123456"
    instance_type = "t2.micro"
  }
  ```

- **Variable**: Declare input variables.  
  ```hcl
  variable "instance_type" {
    description = "Type of instance"
    default     = "t2.micro"
  }
  ```

- **Output**: Declare output values.  
  ```hcl
  output "instance_ip" {
    value = aws_instance.example.public_ip
  }
  ```

### State Management
- **View the current state**:  
  ```bash
  terraform show
  ```

- **List all resources in the state**:  
  ```bash
  terraform state list
  ```

- **Remove a resource from the state**:  
  ```bash
  terraform state rm <resource_address>
  ```

### Workspaces
- **Create a new workspace**:  
  ```bash
  terraform workspace new <workspace_name>
  ```

- **Select a workspace**:  
  ```bash
  terraform workspace select <workspace_name>
  ```

- **List all workspaces**:  
  ```bash
  terraform workspace list
  ```

### Modules
- **Define a module**:  
  ```hcl
  module "example" {
    source = "./module-path"
    instance_type = var.instance_type
  }
  ```


### Helpful Tips
- Use `terraform plan -out=plan.out` to save the execution plan and apply it later with `terraform apply plan.out`.
- Use `terraform graph` to visualize dependencies.



## Getting Started

```tf

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }

  required_version = ">= 1.2.0"
}

provider "aws" {
  region  = "us-west-2"
}

resource "aws_instance" "app_server" {
  ami           = "ami-830c94e3"
  instance_type = "t2.micro"

  tags = {
    Name = "ExampleAppServerInstance"
  }
}
```

**Configuration File:** The core of the process involves creating a Terraform configuration file named "main.tf". This file will contain several code blocks:
- **Terraform Block**: This block specifies the required providers, including their source and version. In this tutorial, it uses the "hashicorp/aws" provider, which is responsible for managing AWS resources.
- **Provider Block**: This block configures the specific provider. In this case, it's the "aws" provider. we can set the region where we want to deploy our resources. This block tells Terraform how to interact with a specific service provider like AWS.
- **Resources Block**: This block defines the components of our infrastructure we want to create. In this case, it defines an EC2 instance (a virtual machine) using the "aws_instance" resource. we provide details like the machine type, AMI ID (Amazon Machine Image), and tags for organization.

**Initialization:** Running the command **"terraform init"** is crucial. This command downloads and installs the necessary provider plugins defined in our configuration. It creates a **".terraform" directory** to store these plugins. It also generates a lock file (".terraform.lock.hcl") to keep track of the specific provider versions used, ensuring consistency.

**Formatting and Validation:** Two more commands help ensure the quality of our configuration:
    - "terraform fmt": This command automatically formats our configuration files, improving readability and consistency.
    - "terraform validate": This command checks if our configuration is syntactically correct and internally consistent, catching potential errors early on.

**Applying the Configuration:** The "**terraform apply**" command is the heart of the process. Terraform analyzes our configuration and generates an execution plan, detailing what actions it will take to create, modify, or delete resources on our AWS account. we get a chance to review this plan before giving our final approval. Once approved, Terraform executes the plan, creating the resources we defined. In this case, it sets up an EC2 instance with the specified settings.

**State Management:** Terraform maintains a state file (**terraform.tfstate**), which is crucial for tracking the resources it manages. This file stores the IDs and properties of our infrastructure components, allowing Terraform to make updates and changes effectively in the future. The tutorial emphasizes the importance of keeping this file secure, as it often contains sensitive information. The command "terraform show" lets we inspect the current state information. we can also list the resources in the state using "**terraform state list**".