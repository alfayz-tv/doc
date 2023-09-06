# install on mac
```sh
# Download the MicroK8s installer
brew install ubuntu/microk8s/microk8s

microk8s install
# or
microk8s install --cpu=4 --mem=10

#Check the status while Kubernetes starts
microk8s status --wait-ready

sudo microk8s --help    
```

```sh
# if got error multipass
multipass stop microk8s-vm
multipass delete microk8s-vm
multipass purge
```