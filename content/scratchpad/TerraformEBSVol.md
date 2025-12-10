```tf
resource "aws_ebs_volume" "datacenter" {
  availability_zone = "us-east-1a"
  size              = 2
  type = "gp3"

  tags = {
    Name = "datacenter-volume"
  }
}
```