main.tf

```tf
resource "aws_vpc" "priv-devops" {
    cidr_block = var.KKE_VPC_CIDR
    instance_tenancy = "default"
    tags = {
        Name = "devops-priv-vpc"
    }
}

resource "aws_subnet" "priv-devops" {
    vpc_id = aws_vpc.priv-devops.id
    cidr_block = var.KKE_SUBNET_CIDR
    map_public_ip_on_launch = false
    tags = {
        Name = "devops-priv-subnet"
    }
}

resource "aws_network_interface" "devops-priv-ec2" {
  subnet_id   = aws_subnet.priv-devops.id
  private_ips = ["10.0.1.2"]

  tags = {
    Name = "primary_network_interface"
  }
}

resource "aws_instance" "priv-devops" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"

  network_interface {
    network_interface_id = aws_network_interface.devops-priv-ec2.id
    device_index         = 0
  }

  lifecycle {
    ignore_changes = [
        network_interface
    ]
 }

  tags = {
    Name = "devops-priv-ec2"
  }

}

resource "aws_security_group" "priv-ec2" {
  tags = {
    type = "priv-ec2-security-group"
  }
}

resource "aws_vpc_security_group_ingress_rule" "allow_tls_ipv4" {
  security_group_id = aws_security_group.priv-ec2.id
  cidr_ipv4         = aws_vpc.priv-devops.cidr_block
  ip_protocol       = "-1"
}

resource "aws_vpc_security_group_ingress_rule" "allow_tls_ipv6" {
  security_group_id = aws_security_group.priv-ec2.id
  cidr_ipv6         = aws_vpc.priv-devops.ipv6_cidr_block
  ip_protocol       = "-1"
}

resource "aws_vpc_security_group_egress_rule" "allow_all_traffic_ipv4" {
  security_group_id = aws_security_group.priv-ec2.id
  cidr_ipv4         = "0.0.0.0/0"
  ip_protocol       = "-1" # semantically equivalent to all ports
}

resource "aws_vpc_security_group_egress_rule" "allow_all_traffic_ipv6" {
  security_group_id = aws_security_group.priv-ec2.id
  cidr_ipv6         = "::/0"
  ip_protocol       = "-1" # semantically equivalent to all ports
}

resource "aws_network_interface_sg_attachment" "priv-ec2" {
  security_group_id    = aws_security_group.priv-ec2.id
  network_interface_id = aws_instance.priv-devops.primary_network_interface_id
}
```


variables.tf
```tf
variable "KKE_VPC_CIDR" {
    type = string
    default = "10.0.0.0/16"
}

variable "KKE_SUBNET_CIDR" {
    type = string
    default = "10.0.1.0/24"
}
```

outputs.tf

```tf
output "KKE_vpc_name" {
    value = aws_vpc.priv-devops.tags.Name
}

output "KKE_subnet_name" {
    value = aws_subnet.priv-devops.tags.Name
}

output "KKE_ec2_private" {
    value = aws_instance.priv-devops.tags.Name
}
```