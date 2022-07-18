# Description
Documentation install Docker under Windows 10

## Dependencies

- Windows 10 Home or later

## Install Kernel WSL Application (Ubuntu 20.04)
```
wsl --install -d Ubuntu
```

## Set Ubuntu App default app if not
```
wsl -s Ubuntu
```

## install some dependencies
```
sudo apt-get update
sudo apt-get install wget
sudo apt-get install curl
```

## Add genie repository
```
sudo su
wget -O /etc/apt/trusted.gpg.d/wsl-transdebian.gpg https://arkane-systems.github.io/wsl-transdebian/apt/wsl-transdebian.gpg

chmod a+r /etc/apt/trusted.gpg.d/wsl-transdebian.gpg

cat << EOF > /etc/apt/sources.list.d/wsl-transdebian.list
deb https://arkane-systems.github.io/wsl-transdebian/apt/ $(lsb_release -cs) main
deb-src https://arkane-systems.github.io/wsl-transdebian/apt/ $(lsb_release -cs) main
EOF

apt update

exit
```

## Install systemd genie
```
sudo apt install -y systemd-genie
```

## Remove systemd errors

Check mount device
```
mount
```

Remove **systemd-remount-fs.service** error check device and execute the next command with the device checked previusly
```
sudo e2label /dev/sdb cloudimg-rootfs
```

Remove **ssh.service** error
```
sudo ssh-keygen -A
```

Remove **multipathd.service** error
```
sudo systemctl disable multipathd.service
```

## Install Docker

```
sudo apt update

sudo apt install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

apt-cache policy docker-ce

sudo apt install docker-ce

sudo systemctl status docker
```

## Configure docker without user

```
sudo usermod -aG docker ${USER}
```

## Start WSL
```
wsl genie -s
```

## Stop WSL
```
wsl --shutdown
```

## Some links
- WSL Genie: https://github.com/arkane-systems/genie
- WSL Genie systemd errors:https://github.com/arkane-systems/genie/wiki/Systemd-units-known-to-be-problematic-under-WSL
- Docker install in Ubuntu 22.04: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04
