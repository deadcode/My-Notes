# How to create a Kubernetes cluster using VMs

Steps to install a full Kubernetes cluster on a local machine using Virtual Machines. The cluster will include 3 nodes:
* 1 Master node
* 2 Worker nodes

The steps should be the same for Linux or Mac. The specific exmaples are for ARM Mac.

Resources:
* VM environment / Hypervisor
  * Oracle VirtualBox (https://www.virtualbox.org/): Works on Windows, Linux or Intel Mac
  * KVM: Works on Linux
  * VMware Fusion (the free version for indivdual use): Works on ARM Mac
* Vagrant to manage and configure the VMs (https://www.vagrantup.com/)
```
brew tap hashicorp/tap
brew install hashicorp/tap/hashicorp-vagrant
```
* Vagrant file to install VMs
  * Use Ubuntu-24.04 as base image
  * Use the file [here](./Vagrantfile)
* Kubernetes docs for installing 'kubeadm' (https://kubernetes.io/docs/setup/independent/install-kubeadm/)

Steps:
* Install Vagrant Vmware connector (https://developer.hashicorp.com/vagrant/docs/providers/vmware/installation)
```
vagrant plugin install vagrant-vmware-desktop
```
* Bring up the VMs
```
$ vagrant status
Current machine states:

kubemaster                not created (vmware_desktop)
kubenode01                not created (vmware_desktop)
kubenode02                not created (vmware_desktop)

This environment represents multiple VMs. The VMs are all listed
above with their current state. For more information about a specific
VM, run `vagrant status NAME`.
$
$ vagrant up
Bringing machine 'kubemaster' up with 'vmware_desktop' provider...
Bringing machine 'kubenode01' up with 'vmware_desktop' provider...
Bringing machine 'kubenode02' up with 'vmware_desktop' provider...
==> kubemaster: Cloning VMware VM: 'bento/ubuntu-24.04'. This can take some time...
==> kubemaster: Verifying vmnet devices are healthy...
==> kubemaster: Preparing network adapters...
...
...
$ 
$ vagrant status
Current machine states:

kubemaster                running (vmware_desktop)
kubenode01                running (vmware_desktop)
kubenode02                running (vmware_desktop)

This environment represents multiple VMs. The VMs are all listed
above with their current state. For more information about a specific
VM, run `vagrant status NAME`.
```

* Follow the steps from the Kubernetes docs
  * Install 'containerd' on the VM nodes
    * https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
    * https://github.com/containerd/containerd/blob/main/docs/getting-started.md
  * Configure the cgroups driver on the VM nodes
    * https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/configure-cgroup-driver/#configuring-the-kubelet-cgroup-driver
  * Install 'kubeadm' on the VM nodes
    * https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
  * Create a cluster with 'kubeadm'
  ```
  kubeadm init
  ```
  * Install the Pod network: A Container Network Interface (CNI) based Pod network add-on so that the Pods can talk to each other.