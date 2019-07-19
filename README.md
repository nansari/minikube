# minikube

## Objective
Create a single page document to experience kubernetes in minikube that can be followed sequentially and completed in couple of hours.

## minikube installation

Follow [Installation document](https://kubernetes.io/docs/tasks/tools/install-minikube/) to install minikube and pre-requisit on your Windows, Linux or MacOS.

## Try minikube commands

Start minikube
```
$ minikube start
😄  minikube v1.0.0 on linux (amd64)
🤹  Downloading Kubernetes v1.14.0 images in the background ...
💡  Tip: Use 'minikube start -p <name>' to create a new cluster, or 'minikube delete' to delete this one.
🔄  Restarting existing virtualbox VM for "minikube" ...
⌛  Waiting for SSH access ...
📶  "minikube" IP address is 192.168.99.100
🐳  Configuring Docker as the container runtime ...
🐳  Version of container runtime is 18.06.2-ce
⌛  Waiting for image downloads to complete ...
✨  Preparing Kubernetes environment ...
🚜  Pulling images required by Kubernetes v1.14.0 ...
🔄  Relaunching Kubernetes v1.14.0 using kubeadm ... 
⌛  Waiting for pods: apiserver proxy etcd scheduler controller dns
📯  Updating kube-proxy configuration ...
🤔  Verifying component health ......
💗  kubectl is now configured to use "minikube"
🏄  Done! Thank you for using minikube!
```

Check status, version and minikube vitual machine IP
```
$ minikube status
host: Running
kubelet: Running
apiserver: Running
kubectl: Correctly Configured: pointing to minikube-vm at 192.168.99.100

$ minikube  ip
192.168.99.100

$ minikube version
minikube version: v1.0.0
```

Check ```minikube logs```. Useful to know minikube health - not user's code error.
```
$ minikube logs | head -5
==> coredns <==
.:53
2019-07-19T08:51:29.410Z [INFO] CoreDNS-1.3.1
2019-07-19T08:51:29.410Z [INFO] linux/amd64, go1.11.4, 6b56a9c
CoreDNS-1.3.1

$ minikube logs --problems
❌  Problems detected in "kube-addon-manager":
    error: no objects passed to apply

```

```minikube logs``` command shows logs of following componenets.
```
$ minikube logs |grep ^==

==> coredns <==
==> dmesg <==
==> kernel <==
==> kube-addon-manager <==
==> kube-apiserver <==
==> kube-proxy <==
==> kube-scheduler <==
==> kubelet <==
==> kubernetes-dashboard <==
==> storage-provisioner <==
```

What are addons minikube has?
```
$ minikube addons list
- addon-manager: enabled
- dashboard: enabled
- default-storageclass: enabled
- efk: disabled
- freshpod: disabled
- gvisor: disabled
- heapster: disabled
- ingress: disabled
- logviewer: disabled
- metrics-server: disabled
- nvidia-driver-installer: disabled
- nvidia-gpu-device-plugin: disabled
- registry: disabled
- registry-creds: disabled
- storage-provisioner: enabled
- storage-provisioner-gluster: disabled
```

Let's try to enable and then disable heapster (used to collect metrics). It is being replaced with metrics-server in newer version of kubernetes
```
$ minikube addons enable heapster
✅  heapster was successfully enabled
$ minikube addons disable heapster
✅  heapster was successfully disabled
$ 
```

Login to minikube Linux VM. Check Linux Virtual machine hostname and IP. This is ip shown by ```minikube ip``` command
```
$ minikube ssh
                         _             _            
            _         _ ( )           ( )           
  ___ ___  (_)  ___  (_)| |/')  _   _ | |_      __  
/' _ ` _ `\| |/' _ `\| || , <  ( ) ( )| '_`\  /'__`\
| ( ) ( ) || || ( ) || || |\`\ | (_) || |_) )(  ___/
(_) (_) (_)(_)(_) (_)(_)(_) (_)`\___/'(_,__/'`\____)

# hostname
minikube

# cat /etc/os-release
NAME=Buildroot
VERSION=2018.05
ID=buildroot
VERSION_ID=2018.05
PRETTY_NAME="Buildroot 2018.05"

# ip addr show|grep -w inet
    inet 127.0.0.1/8 scope host lo
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic eth0
    inet 192.168.99.100/24 brd 192.168.99.255 scope global dynamic eth1
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0

# ps -aef|grep kub|cut -c 1-140
  ....
  truncated - these are componenets of kubernetes master and node
  ....

# kubelet --version
Kubernetes v1.14.0
```

Finally, see what other commands minikube offers using```minikube help``` 

One cluster is not enough for you? Create one more. It will create a new VM in VirtualBox. **_Do it only if you have enough RAM_**
Stop running minikube before creating a new one. The command creates new VM, assign a new IP and configure kubectl CLI default setting to connect to new cluster.
```
$ minikube stop

$ minikube start -p second-cluster
😄  minikube v1.0.0 on linux (amd64)
🤹  Downloading Kubernetes v1.14.0 images in the background ...
🔥  Creating virtualbox VM (CPUs=2, Memory=2048MB, Disk=20000MB) ...
📶  "second-cluster" IP address is 192.168.99.101
🐳  Configuring Docker as the container runtime ...
🐳  Version of container runtime is 18.06.2-ce
⌛  Waiting for image downloads to complete ...
✨  Preparing Kubernetes environment ...
🚜  Pulling images required by Kubernetes v1.14.0 ...
🚀  Launching Kubernetes v1.14.0 using kubeadm ... 
⌛  Waiting for pods: apiserver proxy etcd scheduler controller dns
🔑  Configuring cluster permissions ...
🤔  Verifying component health .....
💗  kubectl is now configured to use "second-cluster"
🏄  Done! Thank you for using minikube!

$ minikube delete -p second-cluster
🔥  Deleting "second-cluster" from virtualbox ...
💔  The "second-cluster" cluster has been deleted.
$
```

Next, try to [use set of kubectl commands to deploy a web server in kubernetes](https://github.com/nansari/kubernetes/blob/master/README.md) running inside minikube

At last ```minikube stop``` if you are done!


