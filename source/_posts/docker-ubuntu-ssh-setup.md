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

**host**: use the same port -> just change to the ssh port that does not contract with nas

## Environment Settings For Ubuntu Container

:::spoiler Dockerfile
```dockerfile=
FROM ubuntu:latest
RUN apt-get update && apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN echo 'root:your_password' | chpasswd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

# Other installation
RUN apt-get upgrade -y
RUN apt-get install sudo
RUN apt-get install nano
RUN apt-get install vim -y
RUN apt-get install curl -y
RUN apt-get install git -y

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
```
:::

:::spoiler Docker build and push to Docker Hub
```shell=
docker build -t ubuntu_ssh .
docker tag ubuntu_ssh wulukewu/ubuntu_ssh:latest
docker push wulukewu/ubuntu_ssh:latest
```
:::

:::spoiler Install OpenSSH-Server
```shell=
apt-get update 
apt-get install -y openssh-server
mkdir /var/run/sshd
echo 'root:your_password' | chpasswd
sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
`/usr/sbin/sshd -D`
```
:::

---

Pull `ubuntu_ssh` From Docker Hub
```shell=
sudo docker pull wulukewu/ubuntu_ssh:latest
```

Run `ubuntu_ssh` in Host
```shell=
sudo docker run -d --net=host --name ubuntu_ssh wulukewu/ubuntu_ssh:latest
```

Get Inside Ubuntu Container
```shell=
sudo docker exec -i -t ubuntu_ssh /bin/bash
```

---

Change Hostname
```shell=
sudo nano /etc/hostname
sudo nano /etc/hosts
sudo reboot
```

Display The Current Ubuntu Hostname
```shell=
hostname
```

---

Change SSH Port
```shell=
sudo nano /etc/ssh/sshd_config
service ssh restart
```

---

Add New Sudo User to the System
```shell=
adduser user_name
```

Adding the User to the sudo Group
```shell=
usermod -aG sudo user_name
usermod -aG root user_name
```

Change user password
```shell=
sudo passwd user_name
```

Testing sudo Access
```shell=
su - user_name
```
---

## References
- [How to SSH into a Docker Container | Step-by-Step Tutorial](https://www.cherryservers.com/blog/ssh-into-docker-container)
- [把image另外儲存成檔案](https://peihsinsu.gitbooks.io/docker-note-book/content/docker-save-image.html)
- [Ubuntu Linux Change Hostname (computer name)](https://www.cyberciti.biz/faq/ubuntu-change-hostname-command/)
- [Day 21：介紹 Docker 的 Network (二)](https://ithelp.ithome.com.tw/articles/10193457)
- [[小抄] Docker 基本命令](https://yingclin.github.io/2018/docker-basic.html)
- [How To Create A New Sudo Enabled User on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-create-a-new-sudo-enabled-user-on-ubuntu)
- [Change password on root user and user account](https://askubuntu.com/questions/423942/change-password-on-root-user-and-user-account)
- [Ask ChatGPT](https://chatgpt.com/share/673dd661-36a0-8007-8442-b5d1cb04bebd)