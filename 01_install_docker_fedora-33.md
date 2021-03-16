```bash
# Base Configuration
1. installe Fedora-33-WS
2. configured network and route.

# ip configuration for interface
ifconfig eth0 192.168.2.21 netmask 255.255.255.0 up

# configuration updated in file
vi /etc/sysconfig/network-scripts/ifcfg-eth0

# Default route
sudo route add default gw 192.168.2.2

# DNS configuration
echo "nameserver 8.8.8.8" > /etc/resolv.conf
-----------------------------------------------------------------------

# Installing Docker of Fedora 33

Commands:
# add repo and install CE edition

sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
sudo dnf install -y docker-ce docker-ce-cli containerd.io

# CGroups Backwards Compatibility
sudo grubby --update-kernel=ALL --args="systemd.unified_cgroup_hierarchy=0"
  # <reboot after this>

# Run hello-world to confirm install
sudo systemctl start docker
sudo docker run hello-world

# Setup a non-root user to use docker
sudo usermod -aG docker jay
newgrp docker

# Run from non-root user
docker run hello-world
```