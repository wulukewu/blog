---
title: Docker - Ubuntu SSH Setup
date: 2024-11-20 20:51:19
tags:
    - docker
    - ubuntu
    - ssh
    - devops
    - containerization
    - linux
    - tutorial
---

# Docker - Ubuntu SSH Setup

**Host**: Use the same port -> just change to the SSH port that does not conflict with NAS.

## Environment Settings for Ubuntu Container

### Dockerfile
```dockerfile
FROM ubuntu:latest
RUN apt-get update && apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN echo 'root:your_password' | chpasswd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

# Other installations
RUN apt-get upgrade -y
RUN apt-get install -y sudo nano vim curl git

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
```

### Docker Build and Push to Docker Hub
```shell
docker build -t ubuntu_ssh .
docker tag ubuntu_ssh wulukewu/ubuntu_ssh:latest
docker push wulukewu/ubuntu_ssh:latest
```

### Install OpenSSH-Server
```shell
apt-get update 
apt-get install -y openssh-server
mkdir /var/run/sshd
echo 'root:your_password' | chpasswd
sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
/usr/sbin/sshd -D
```

---

## Pull and Run the ubuntu_ssh Image

### Pull ubuntu_ssh from Docker Hub
```shell
sudo docker pull wulukewu/ubuntu_ssh:latest
```

### Run ubuntu_ssh in Host
```shell
sudo docker run -d --net=host --name ubuntu_ssh wulukewu/ubuntu_ssh:latest
```

### Access the Ubuntu Container
```shell
sudo docker exec -i -t ubuntu_ssh /bin/bash
```

---

## Hostname Configuration

### Change Hostname
```shell
sudo nano /etc/hostname
sudo nano /etc/hosts
sudo reboot
```

### Display the Current Hostname
```shell
hostname
```

---

## SSH Configuration

### Change SSH Port
```shell
sudo nano /etc/ssh/sshd_config
service ssh restart
```

---

## User Management

### Add a New Sudo User
```shell
adduser user_name
```

### Add the User to the Sudo Group
```shell
usermod -aG sudo user_name
usermod -aG root user_name
```

### Change User Password
```shell
sudo passwd user_name
```

### Test Sudo Access
```shell
su - user_name
```

---

## References
- [How to SSH into a Docker Container | Step-by-Step Tutorial](https://www.cherryservers.com/blog/ssh-into-docker-container)
- [Save Docker Image as a File](https://peihsinsu.gitbooks.io/docker-note-book/content/docker-save-image.html)
- [Ubuntu Linux Change Hostname](https://www.cyberciti.biz/faq/ubuntu-change-hostname-command/)
- [Day 21: Introduction to Docker Networking (Part 2)](https://ithelp.ithome.com.tw/articles/10193457)
- [Docker Basic Commands Cheat Sheet](https://yingclin.github.io/2018/docker-basic.html)
- [How to Create a New Sudo-Enabled User on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-create-a-new-sudo-enabled-user-on-ubuntu)
- [Change Password for Root and User Accounts](https://askubuntu.com/questions/423942/change-password-on-root-user-and-user-account)
- [Ask ChatGPT](https://chatgpt.com/share/673dd661-36a0-8007-8442-b5d1cb04bebd)