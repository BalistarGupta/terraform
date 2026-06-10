resource "aws_instance" "balifirst" {
  ami               = "ami-00a3ff43223e36738"
  availability_zone = "ap-south-1a"
  instance_type     = "t3.micro"
  key_name          = "K8Skey"
  user_data         = <<EOF
#!/bin/bash

sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
echo "<h1>sample webserver using terraform</h1>" | sudo tee /var/www/html/index.html
EOF
  tags = {
    Name = "test_machine"
  }
}
~
