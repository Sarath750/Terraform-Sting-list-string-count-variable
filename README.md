# Terraform-Sting-list-string-count-variable

Creating 2 instances with different availability zones, tags etc.,

provider.tf
-----------
provider "aws" {
  region     = "us-east-1"
  version = "~> 4.0"
  access_key = "AKIAS5QJFNKWB2YIVC22"
  secret_key = "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"
} 


aws_instance.tf
---------------
resource "aws_instance" "web" {
  ami           = var.ami-id
  instance_type = var.instance_type
  key_name   = var.key_name 
  count             = 2
  availability_zone = element(var.availability_zones, count.index)

  tags = {
    Name = element(var.instance_tags, count.index)
  }
}


variables.tf
------------
variable "ami-id" {
  type    = string
  default = "ami-090fa75af13c156b4"
}

variable "instance_type" {
  type    = string
  default = "t2.micro"
}

variable "key_name" {
  type    = string
  default = "8ambatch"
}

variable "availability_zones" {
  type    = list(string)
  default = ["us-east-1a","us-east-1b"]
}

variable "instance_tags" {
  type = list(string)
  default = ["HelloWorld-1","HelloWorld-2"]
}

terraform apply -auto-approve


For list(string) we should use element and count.index
