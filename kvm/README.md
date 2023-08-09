## KVM
KVM stands for Kernel-based Virtual Machine which is an open source virtualization technology built into Linux. That means a Linux host can be turned into a *Hypervisor* that runs multiple isolated virtual environments.

### How to install KVM on Debian?
*Information was extracted from https://wiki.debian.org/KVM#Installation*

If you are installing KVM on a workstation:
`$ sudo apt install qemu-system libvirt-daemon-system virt-manager`

If you are install KVM on a server (without GUI):
`$ sudo apt install --no-install-recommends qemu-system libvirt-clients libvirt-daemon-system`

Once installed, you can add current user to `libvirt` group to manage virtual machine as non-root user.
`adduser <usernam> libvirt`


### How to create a KVM guest?
*The following references are for more advanced setup using Ansible or Terraform. We will be discussing the relevant topic in a separate guide*
- https://blog.christophersmart.com/2020/03/03/using-ansible-to-define-and-manage-kvm-guests-and-networks-with-yaml-inventories/
- https://www.redhat.com/sysadmin/build-VM-fast-ansible
- https://medium.com/@art.vasilyev/use-ubuntu-cloud-image-with-kvm-1f28c19f82f8

1. Download a cloud image of your preferred distro. In this case, we download a Ubuntu cloud image.
`wget https://cloud-images.ubuntu.com/jammy/20230805/jammy-server-cloudimg-amd64.img`

2. Create a directory to store the downloaded base image.
`mkdir -p /var/lib/libvirt/images/base; cd /var/lib/libvirt/images/base; mv <image> <image.qcow2>` 

3. Create a director to store KVM instance image. The new KVM instance image can be created based off the base image. The new image can be resized by specifying the desired virtual disk size.
`mkdir -p /var/lib/libvirt/images/ubuntu; cd /var/lib/libvirt/images/ubuntu; qemu-img create -b ../base/image.qcow2 -F qcow2 -f qcow2 instance.qcow2 70G`

4. Before deploying the KVM instance, we can use `Cloud-Init` to configure the user data of KVM image
```bash
#cloud-config
hostname: instance1 # set hostname
fqdn: instance1
manage_etc_hosts: falsessh_pwauth: true
disable_root: false
users:
  - default
  - name: ubuntu
    shell: /bin/bash
    sudo: ALL=(ALL) NOPASSWD:ALL
    lock_passwd: false
    ssh-authorized-keys:
      - "path/to/pubkey"
chpasswd:
  list: |
    root:password
    ubuntu:password
  expire: false
runcmd:
  - [ sh, -c, echo 192.168.100.10 cl-ubuntu | tee -a /etc/hosts]
```

5. Create a network configuration to assign static IP address to the KVM instance.
```bash
version: 2
ethernets:
  ens3:
    dhcp4: false
    addresses: [ 192.168.122.222/24 ]
    gateway4: 192.168.122.1
    nameservers:
      addresses: [ 192.168.122.1 ]
```

6. Generate ISO image with the user data and network configuration
`cloud-localds -v --network-config=network.yaml instance.qcow2 userdata.yaml`

7. Provision the KVM VM
`virt-install --connect qemu:///system --virt-type kvm --name instance1 --ram 1024 --vcpus=1 --os-variant <os-variant> --disk path=/var/lib/libvirt/images/ubuntu/instance.qcow2,format=qcow2 --disk /var/lib/libvirt/images/ubuntu/instance.iso,device=cdrom --import --network network=default --noautoconsole`

### Useful commands to manage virtual machines
1. List of VMs managed by libvirt
`virsh list --all`

2. Show IP Address of a KVM VM instance
`virsh domifaddr <VM>`

3. Start or shutdown a VM
`virsh start | shutdown <VM>`

4. Start a network
`virsh net-start <network>`