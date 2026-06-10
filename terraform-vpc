resource "aws_vpc" "bali-vpc" {
  cidr_block = "10.0.0.0/16"
  tags = {
    Name   = "production"
    source = "terraform"
  }
}

#creating Internet Gateway and attach it to vpn
resource "aws_internet_gateway" "bali-IGW" {
  vpc_id = aws_vpc.bali-vpc.id
  tags = {
    Name   = "production"
    source = "terraform"
  }
}

#create public subnets
resource "aws_subnet" "public" {
  vpc_id            = aws_vpc.bali-vpc.id
  availability_zone = "ap-south-1a"
  cidr_block        = "10.0.1.0/24"
  tags = {
    Name   = "production"
    source = "terraform"
  }
}
#create private subnet
resource "aws_subnet" "private" {
  vpc_id            = aws_vpc.bali-vpc.id
  availability_zone = "ap-south-1b"
  cidr_block        = "10.0.2.0/24"
  tags = {
    name   = "production"
    source = "terraform"
  }
}
#route table for public subnets
resource "aws_route_table" "bali-route-table" {
  vpc_id = aws_vpc.bali-vpc.id
  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.bali-IGW.id
  }
  tags = {
    Name   = "production"
    source = "terraform"
  }
}
#Route table association with public subnets
resource "aws_route_table_association" "publicRTassocoation" {
  subnet_id      = aws_subnet.public.id
  route_table_id = aws_route_table.bali-route-table.id
}
#security group
resource "aws_security_group" "bali-vpc-sg" {
  name        = "bali-vpc-sg"
  vpc_id      = aws_vpc.bali-vpc.id
  description = "allow ssh and http"

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
#ec2 create
resource "aws_instance" "linux" {
  ami                         = "ami-00a3ff43223e36738"
  instance_type               = "t3.micro"
  subnet_id                   = aws_subnet.public.id
  vpc_security_group_ids      = ["${aws_security_group.bali-vpc-sg.id}"]
  associate_public_ip_address = true
  key_name                    = "K8Skey"
  tags = {
    Name   = "production"
    source = "terraform"
  }

}
