# rke-ansible

This repo contains an ansible playbook and roles for installing
Rancher's RKE based kubernetes cluster.

## Instructions

### Create Linux VMs for the Kubernetes cluster

You will need some Linux VMs to run your RKE cluster on.  You can 
create these however you like (AWS, GCP, Openstack).

### Create a Linux VM for Ansible

#### Copy your private key to the ansible VM

I copied my private key `id_rsa` to this directory on the ansible VM.

```angular2html
ubuntu@ansible:~/.ssh$ ls
authorized_keys  id_rsa  id_rsa.pub  known_hosts
```

#### Add your RKE IPs to /etc/hosts

Add your RKE VM IPs to your `/ect/hosts` file on your ansible VM.

Mine looks like this:

```angular2html
ubuntu@ansible:~/.ssh$ cat /etc/hosts
127.0.0.1 localhost
192.168.192.26 kube-1
192.168.192.25 kube-2
192.168.192.22 kube-3
192.168.192.14 kube-4
192.168.192.23 kube-5

```

#### Run the playbook

Run this command on the ansible VM:

```angular2html
ubuntu@ansible:~/rke-ansible$ 
export ANSIBLE_HOST_KEY_CHECKING=False
ansible-playbook -i hosts.ini rke-playbook.yml -v
```

After this runs your RKE Kubernetes cluster should be running.
To verify the cluster is running ssh to the Kubernetes master vm and run `kubectl get nodes`.

Root up and run this on kube-1:

```ubuntu@kube-1:~$ sudo su -
root@kube-1:~# kubectl get nodes
NAME     STATUS   ROLES               AGE     VERSION
kube-1   Ready    controlplane,etcd   8m33s   v1.19.3
kube-2   Ready    controlplane,etcd   8m32s   v1.19.3
kube-3   Ready    worker              8m27s   v1.19.3
kube-4   Ready    worker              8m28s   v1.19.3
kube-5   Ready    worker              8m26s   v1.19.3
```

