==========write 20 ec2 instances=======
# Define 20 EC2 instances
resource "aws_instance" "instances" {
  count         = 20
  ami           = "ami-0c55b159cbfafe1f0" # Replace with your desired AMI ID
  instance_type = "t2.micro"                # Replace with your desired instance type
  key_name      = aws_key_pair.example.key_name
  security_groups = [aws_security_group.example.name]

  tags = {
    Name = "example-instance-${count.index}"
  }

  # Optional: Add block storage
  ebs_block_device {
    device_name = "/dev/xvda"
    volume_size = 8 # Adjust as needed
  }


===============write script 20 s3 bucket in terraform add different roles================
provider "aws" {
  region = "us-east-1"
}

# Define the S3 buckets
resource "aws_s3_bucket" "buckets" {
  count = 20
  bucket = "my-bucket-${count.index}"
  acl    = "private"

  tags = {
    Name = "my-bucket-${count.index}"
  }
}

# Define IAM roles
resource "aws_iam_role" "role1" {
  name = "role1"

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action    = "sts:AssumeRole"
        Effect    = "Allow"
        Principal = {
          Service = "ec2.amazonaws.com"
        }
      },
    ]
  })
}
