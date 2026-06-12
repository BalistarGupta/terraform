resource "aws_instance" "main" {
  ami           = "ami-00a3ff43223e36738"
  instance_type = "t3.micro"
  key_name      = aws_key_pair.my_blog_key.key_name
  connection {
    type        = "ssh"
    host        = self.public_ip
    user        = "ec2-user"
    private_key = file("//home/ec2-user/project/ram")

  }

  provisioner "file" {
    source      = "/tmp/redhat.txt"
    destination = "/tmp/love.txt"

  }

  provisioner "local-exec" {
    command = "echo ${aws_instance.main.private_ip}  >>/root/check_ip.txt"

  }

  provisioner "remote-exec" {
    inline = [
      "touch hellow.txt",
      "echo helloworld remote provisioner >> hello.txt",
    ]

  }

  tags = {
    Name   = "production"
    source = "terraform"

  }
}

