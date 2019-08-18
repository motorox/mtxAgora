#Kyma install steps

### Debian 10 as OS

### HW
Lenovo T470
- https://cdimage.debian.org/cdimage/unofficial/non-free/images-including-firmware/current/amd64/iso-dvd/ - ISO image with firmware and nonfree
- https://packages.debian.org/buster/firmware-misc-nonfree
```
apt-get install firmware-misc-nonfree
```

### OS 
ATTENTION -> VirtualBox not supported on Buster(10) or SID (9)

- https://docs.docker.com/install/linux/docker-ce/debian/ - docker installation
- https://minikube.sigs.k8s.io/docs/start/linux/ - minikube install
- https://wiki.debian.org/KVM#Installation - kvm installation
- https://helm.sh/docs/using_helm/#installing-helm - helm install, chose from Script
- https://fabianlee.org/2019/02/11/kubernetes-running-minikube-locally-on-ubuntu-using-kvm/ - as the installation of minikube with kvm didnt work, do the steps from this site
- https://cravencode.com/post/kubernetes/setup-minikube-on-ubuntu-kvm2/ - here you can find how to make it work with kvm2 (delete and restart minikube)

Check the virtualization:
'''
virt-host-validate -> and check if there is something wrong. I had the intel_iommu not enabled in the kernel.
nano /etc/default/grub
add @ variable GRUB_CMDLINE_LINUX_DEFAULT the following: intel_iommu=on
run update-grub
'''

- https://github.com/kyma-project/cli - kyma-cli install
- https://kyma-project.io/docs/root/kyma#installation-installation - Latest step: kyma install
'''
kyma provision minikube
minikube start
kyma install
'''
both kyma commands probably will need sudo rights. Do it with the following:
'''
#usermod -aG sudo <username>
'''


### Links

- https://kyma-project.io/
- https://github.com/kyma-project
- https://github.com/kubernetes/minikube
- https://github.com/kubernetes/kubectl
- https://github.com/helm/helm
