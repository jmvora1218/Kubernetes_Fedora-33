``` bash

# Install snapd
Snaps are applications packaged with all their dependencies to run on all popular Linux distributions from a single build. They update automatically and roll back gracefully.

sudo dnf install snapd

To enable classic snap support, enter the following to create a symbolic link between /var/lib/snapd/snap and /snap:

sudo ln -s /var/lib/snapd/snap /snap

# Install kustomize

sudo snap install kustomize

# Install minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-latest.x86_64.rpm

sudo rpm -ivh minikube-latest.x86_64.rpm

minikube start

# Install Skaffold 
curl -Lo skaffold https://storage.googleapis.com/skaffold/releases/latest/skaffold-linux-amd64

sudo install skaffold /usr/local/bin/


# Install kubectl
sudo snap install kubectl --classic

# Reffered URL's
https://snapcraft.io/install/kustomize/fedora
https://www.tutorialworks.com/kubernetes/fedora-dev-setup/
https://snapcraft.io/install/kubectl/fedora
https://skaffold.dev/docs/install/

```