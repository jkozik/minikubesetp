# minikube setup notes
```
https://kubernetes.io/docs/tasks/tools/install-minikube/
https://minikube.sigs.k8s.io/docs/start/
https://backstage.forgerock.com/docs/forgeops/6.5/devops-guide-minikube/
https://kubernetes.io/docs/setup/learning-environment/minikube/
https://kubernetes.io/docs/tutorials/hello-minikube/
```

Setup Docker on my dell2 host.  Not in a VM ! First in root login:
```
 # yum install -y yum-utils device-mapper-persistent-data lvm2
 # yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
 # yum install git docker
 # git --version
 # systemctl start docker
 # systemctl enable docker
 # systemctl status docker
 # docker run hello-world
 # docker ps
 # docker ps -a
 # curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
 # chmod +x /usr/local/bin/docker-compose
 # docker-compose --version
 # groupadd docker
 # usermod -aG docker jkozik
 # chmod 666 /var/run/docker.sock
```
Install kubectl (root)
```
# cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
# yum install -y kubectl
# kubectl version –client
```
Next, install minikube (root) 
```
# curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64   && chmod +x minikube
# mkdir -p /usr/local/bin/
# install minikube /usr/local/bin/
# minikube --help
```
Now login with a user id with docker permissions and create a cluster.  The cluster is running in a VirtualBox VM
```
$ minikube start --memory=8192 --disk-size=40g  --vm-driver=virtualbox --bootstrapper kubeadm --kubernetes-version=1.17.4
$ minikube status
$ minikube ssh /bin/bash
  # docker ps     # to see all the containers running in the minikube VM
$ minikube ip     # to see the IP address of the cluster  
$ minikube stop/start/delete
```
It is good to know that minikube mounts the /home directory of the host.  So the minikube VM can easily access host files as follows:
```
# Minikube host prompt
$ minikube ssh
  $ cd /hosthome
  $ ls wjr/public_html       #this is where my realtime.txt weather file is located.
```
Some other useful commands -- that must be run on the host desktop.  For me this means running "Remote Desktop" from my Windows PC. 

To make a kubernetes deployment visible to the outside world, one needs to apply a service to it.  minikube can list the exposed services and can bring up a webpage.
```
$ minikube get services
$ minikube service nwcom-service
```
Also, the kubernetes dashboard is accessible from the Remote Desktop 
```
$ minikube dashboard
```
  
