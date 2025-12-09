```tf
resource "aws_key_pair" "devops" {
    key_name = "devops-kp"
    public_key = tls_private_key.pk.public_key_openssh
}
 
resource "tls_private_key" "pk" {
    algorithm = "RSA"
    rsa_bits = "4096"
}
 
resource "local_file" "ssh_key" {
    filename = "/home/bob/${aws_key_pair.devops.key_name}.pem"
    content = tls_private_key.pk.private_key_pem
    file_permission = "0400"
}
 
resource "aws_default_vpc" "default" {
  tags = {
    Name = "Default VPC"
  }
}
 
resource "aws_default_security_group" "default" {
  vpc_id = aws_default_vpc.default.id
 
  ingress {
    protocol  = -1
    self      = true
    from_port = 0
    to_port   = 0
  }
 
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
 
resource "aws_instance" "devops" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  key_name = aws_key_pair.devops.key_name
  vpc_security_group_ids = [aws_default_security_group.default.id]
 
  tags = {
    Name = "devops-ec2"
  }
}
```
