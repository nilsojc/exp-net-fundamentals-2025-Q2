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

