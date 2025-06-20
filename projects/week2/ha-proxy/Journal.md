## Setting Up Container Lab 

To make the HA-proxy work, container lab is needed to be installed.

```sh
echo "deb [trusted=yes] https://netdevops.fury.site/apt/ / " | sudo tee —a /etc/apt/sources.list.d/netdevops.list
sudo apt update
sudo apt install containerlab
```

I had to get all the bash scripts from scratch using Deepseek LLM to make the HA-proxy work seamlessly with Docker.

```sh
#!/bin/bash

# Update package lists and install essential packages
apt-get update
apt-get install -y openssh-server sudo bash nano

# Prepare SSH directory and enable password authentication
mkdir -p /run/sshd
sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config

# Create user with bash as default shell
useradd -m -s /bin/bash demo
echo 'demo:demo' | chpasswd
usermod -aG sudo demo  # Fixed the typo from 'username' to 'usermod'

# Create a basic .bashrc file for demo user
cat > /home/demo/.bashrc << 'EOF'
export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
alias ls='ls --color=auto'
alias ll='ls -la'
EOF

# Set proper ownership
chown demo:demo /home/demo/.bashrc
``` 

This is one of the setup scripts included in the repository that will be used from this lab, including other scripts.

Then I proceeded to install docker.

```sh
sudo snap install docker
```

After installing, I created the files in the main directory for the ubuntu machine on AWS.

```sh
 sudo touch haproxylab.clab.yaml
 sudo touch proxy-startup.sh
 sudo touch replace-index.sh
 sudo touch setup-ssh.sh
 sudo touch web1.html
 sudo touch web2.html
 sudo touch haproxy-new.cfg
 ```
 
`Containerlab` will be now be used for deployment and the containers for `web1`, `web2` and `ha-proxy` will be created.

```sh
containerlab deploy
```

I noticed then that SSH access is not set up. To troubleshoot this, it is necessary to attach the container to the terminal and access it (in this case ha-proxy container) and then start the ssh service

```sh
sudo docker exec -it 00668f23b70f /bin/bash
service ssh start
```

Checked the status of ssh.

```sh
service ssh status
```

To verify the configuration for our ha proxy I used command `cat /etc/haproxy/haproxy-new.cfg`.

Then, I inspected the status of the containers for the haproxer on container lab.

```sh
sudo containerlab inspect haproxylab.clab.yaml
```



