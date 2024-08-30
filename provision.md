3.

Sure, let’s go through the provisioning process in Terraform step-by-step:

### 1. **Generate SSH Key Pair**

First, generate the SSH key pair that you'll use to connect to your AWS instance:

```bash
ssh-keygen -t rsa -b 2048 -f dove-key
```

This creates two files:
- `dove-key` (private key)
- `dove-key.pub` (public key)

### 2. **Write `providers.tf`**

This file will configure the providers for Terraform. For AWS, it looks like this:

```hcl
provider "aws" {
  region = "us-west-2" # Update this to your preferred region
}
```

### 3. **Write `vars.tf`**

Define your variables here:

```hcl
variable "PRIV_KEY_PATH" {
  default = "dove-key"
}

variable "PUB_KEY_PATH" {
  default = "dove-key.pub"
}

variable "USER" {
  default = "ubuntu"
}

variable "AMIS" {
  type = map(string)
}

variable "REGION" {
  default = "us-west-2"
}

variable "ZONE1" {
  default = "us-west-2a"
}
```

### 4. **Write `instances.tf`**

Define the AWS resources and provisioning logic here:

```hcl
resource "aws_key_pair" "dove-key" {
  key_name   = "dovekey"
  public_key = file(var.PUB_KEY_PATH)
}

resource "aws_instance" "intro" {
  ami             = var.AMIS[var.REGION]
  instance_type   = "t2.micro"
  availability_zone = var.ZONE1
  key_name        = aws_key_pair.dove-key.key_name
  vpc_security_group_ids = ["sg-833e24fd"]

  provisioner "file" {
    source      = "web.sh"
    destination = "/tmp/web.sh"

    connection {
      user        = var.USER
      private_key = file(var.PRIV_KEY_PATH)
      host        = self.public_ip
    }
  }

  provisioner "remote-exec" {
    inline = [
      "chmod u+x /tmp/web.sh",
      "sudo /tmp/web.sh"
    ]
  }
}
```

### 5. **Create Provisioning Scripts**

Write your `web.sh` script that will be executed on the instance. For example:

```bash
#!/bin/bash
echo "Hello, World!" > /var/www/html/index.html
```

### 6. **Apply Changes**

Run Terraform commands to apply your configuration:

1. **Initialize Terraform**:

   ```bash
   terraform init
   ```

2. **Plan the changes**:

   ```bash
   terraform plan
   ```

3. **Apply the changes**:

   ```bash
   terraform apply
   ```

### **Summary**

1. **Generate SSH Key Pair**: Create the SSH key pair to access your instances.
2. **Configuration Files**:
   - **`providers.tf`**: Set up AWS provider.
   - **`vars.tf`**: Define all the variables.
   - **`instances.tf`**: Configure AWS resources and provisioning logic.
3. **Provisioning**:
   - **File Provisioner**: Upload files to the instance.
   - **Remote-exec Provisioner**: Execute commands on the instance.
4. **Apply**: Run Terraform commands to provision and configure your infrastructure.

This workflow sets up your environment, provisions the required resources, and runs configuration scripts on your AWS instance.