1..

Great! Yahan tumhare video ke steps ka ek step-by-step tutorial hai jo tumhe hands-on practice karne mein madad karega:

---

### Step-by-Step Tutorial for Terraform Project

#### 1. **Install Terraform**
   - **Command**: `choco install terraform`
   - **Description**: Chocolatey package manager ke through Terraform install karo.

#### 2. **Create IAM User**
   - **Action**:
     - AWS Management Console mein jao.
     - **IAM** section mein jaake **Users** tab mein **Add User** pe click karo.
     - **Username**: `terraadmin`
     - **Access Type**: Programmatic access
     - **Permissions**: 
       - **Attach existing policies directly** pe click karo.
       - **AdministratorAccess** policy ko select karo.
     - **Create User**: Next, next karke user create karo.
     - **Download CSV**: Access key aur secret key download karo.

#### 3. **Configure AWS CLI**
   - **Command**: `aws configure`
   - **Details**:
     - **Access Key**: Downloaded CSV se copy karke paste karo.
     - **Secret Key**: CSV se copy karke paste karo.
     - **Default region name**: `us-east-2`
     - **Default output format**: `json`

#### 4. **Set Up Terraform Directory**
   - **Commands**:
     - `mkdir terraform-scripts`
     - `cd terraform-scripts`
     - `mkdir exercise1`
   - **Description**: Ek directory `terraform-scripts` banao aur uske andar `exercise1` folder banao.

#### 5. **Create Terraform Configuration File**
   - **File Name**: `instance.tf`
   - **Description**: `instance.tf` file banake usmein apni Terraform configuration likho.
     ```hcl
     # Example content of instance.tf
     provider "aws" {
       region = "us-east-2"
     }

     resource "aws_instance" "example" {
       ami           = "ami-0c55b159cbfafe1f0" # Replace with a valid AMI ID
       instance_type = "t2.micro"
     }
     ```

#### 6. **Run Terraform Commands**
   - **Commands**:
     - `terraform init` - Terraform project ko initialize karo.
     - `terraform plan` - Plan ko validate karo aur changes dekho.
     - `terraform apply` - Changes ko apply karo aur resources create karo.

---

Is tutorial ko follow karke tum video ke steps ko implement kar sakti ho. Agar kisi step mein koi confusion ho, toh bata do!

2.

Yahan tumhare dusre tutorial ke steps hain jo tumhe variables aur AWS resources ke saath Terraform ka use karne mein madad karenge:

---

### Step-by-Step Tutorial for Terraform with Variables

#### 1. **Create `variables.tf` File**
   - **File Name**: `variables.tf`
   - **Description**: Is file mein variables define karo, including region, zones, aur AMI.
   - **Example Content**:
     ```hcl
     variable "region" {
       description = "AWS region"
       type        = string
       default     = "us-east-2"
     }

     variable "zones" {
       description = "List of availability zones"
       type        = list(string)
       default     = ["us-east-2a", "us-east-2b"]
     }

     variable "ami" {
       description = "Map of AMIs for each zone"
       type        = map(string)
       default     = {
         "us-east-2a" = "ami-0c55b159cbfafe1f0",
         "us-east-2b" = "ami-0d5d9d301c853c8f7"
       }
     }
     ```

#### 2. **Create `provider.tf` File**
   - **File Name**: `provider.tf`
   - **Description**: AWS provider ko configure karo.
   - **Example Content**:
     ```hcl
     provider "aws" {
       region = var.region
     }
     ```

#### 3. **Create `instance.tf` File**
   - **File Name**: `instance.tf`
   - **Description**: AWS instance resource ko define karo aur variables ka use karo.
   - **Example Content**:
     ```hcl
     resource "aws_instance" "example" {
       ami           = var.ami[var.zones[0]]  # Using AMI for the first zone
       instance_type = "t2.micro"
       availability_zone = var.zones[0]

       key_name = "VPS"  # Replace with your key pair name
       security_groups = ["default"]  # Replace with your security group

       tags = {
         Name    = "ExampleInstance"
         Project = "MyTerraformProject"
       }
     }
     ```

#### 4. **Initialize Terraform**
   - **Command**: `terraform init`
   - **Description**: Terraform project ko initialize karo.

#### 5. **Validate Configuration**
   - **Command**: `terraform validate`
   - **Description**: Configuration file ko validate karo aur syntax errors check karo.

#### 6. **Format Configuration**
   - **Command**: `terraform fmt`
   - **Description**: Configuration files ko standard format mein convert karo.

#### 7. **Plan Changes**
   - **Command**: `terraform plan`
   - **Description**: Changes ka plan dekho aur ensure karo ki sab kuch sahi hai.

#### 8. **Apply Changes**
   - **Command**: `terraform apply`
   - **Description**: Changes ko apply karo aur resources create karo.

#### 9. **Verify in AWS Console**
   - **Action**: AWS Management Console mein jaake resources ko check karo aur ensure karo ki instance create hua hai.

---

Is tutorial ke steps ko follow karke tum Terraform ke saath variables aur AWS resources ko manage kar sakti ho. Agar koi confusion ho toh zaroor batao!