# opennebula-automation
First step:

Remember: You must install the libvirt, ansible and git packages for the instalation

Version of packages:

libvirt 8
ansible 2.10


Create the template Download the ubuntu  22 for template configuration


inside of the util directory there are some files to help with:


netlab1.xml: to configure the invernal libvirt network:
```
virsh net-import netlab1.xml
```
```
bash virt_install.sh
```

the script to run and prepare the template vm, this step is interactive with the terminal to configure everything. e.g: name, ip, hostname…


Inside of inventory/host_vars/localhost.yml you must to setup some variables(it's look like this:)

```
---
# DNS Session
domain: local.lab
dnsconfig: ok

# Kvm session
libvirt_dir: "/var/lib/libvirt"
img_template: "ubuntu22.04-2"
template_address: "192.168.200.30"


osvariant: ubuntu22.04
vm:
  - name: master
    cpu: 2
    mem: 2048
    net_type: "network"
    net_connector: "netlab1"
    net:
      ip: 192.168.200.10
      mask: 255.255.255.0
      gateway: 192.168.200.1
      dns: 192.168.200.1

    net_connector1: "netlab1"
    net:
      ip: 192.168.200.11
      mask: 255.255.255.0
      gateway: 192.168.200.1
      dns: 192.168.200.1

    net_connector2: "netlab1"
    net:
      ip: 192.168.200.12
      mask: 255.255.255.0
      gateway: 192.168.200.1
      dns: 192.168.200.1

    net_connector3: "netlab1"
    net:
      ip: 192.168.200.13
      mask: 255.255.255.0
      gateway: 192.168.200.1
      dns: 192.168.200.1

  - name: slave
    cpu: 2
    mem: 2048
    net_type: "network"
    net_connector: "netlab1"
    net:
      ip: 192.168.200.14
      mask: 255.255.255.0
      gateway: 192.168.200.1
      dns: 192.168.200.1

    net_connector1: "netlab1"
    net:
      ip: 192.168.200.15
      mask: 255.255.255.0
      gateway: 192.168.200.1
      dns: 192.168.200.1

    net_connector2: "netlab1"
    net:
      ip: 192.168.200.16
      mask: 255.255.255.0
      gateway: 192.168.200.1
      dns: 192.168.200.1

    net_connector3: "netlab1"
    net:
      ip: 192.168.200.17
      mask: 255.255.255.0
      gateway: 192.168.200.1
      dns: 192.168.200.1

    net_connector3: "netlab1"
  - name: fe
    cpu: 2
    mem: 2048
    net_type: "network"
    net_connector: "netlab1"
    net:
      ip: 192.168.200.18
      mask: 255.255.255.0
      gateway: 192.168.200.1
      dns: 192.168.200.1

    net_connector1: "netlab1"
    net:
      ip: 192.168.200.19
      mask: 255.255.255.0
      gateway: 192.168.200.1
      dns: 192.168.200.1

    net_connector2: "netlab1"
    net:
      ip: 192.168.200.20
      mask: 255.255.255.0
      gateway: 192.168.200.1
      dns: 192.168.200.1

    net_connector3: "netlab1"
    net:
      ip: 192.168.200.21
      mask: 255.255.255.0
      gateway: 192.168.200.1
      dns: 192.168.200.1

```





img_template: the template image name without extension template_address: the ip address set to the interface


for vm section you must inform ram, vcpu and net_type for networking selection(bridge or virtual network), and net_connector is the network name for virtual network or the bridge name for brige.




Deploying the vms

clone the repository and enter into the ubuntu directory
```
git clone https://github.com/javad-nemati/opennebula-automation.git
```
```
cd create-vm-kvm/ubuntu
ansible-playbook -i inventory/hosts create-vm-kvm/ubuntu_vms-4nic.yml
```
you can destroy the vms with :
```
ansible-playbook -i inventory/hosts create-vm-kvm/destroy.yml
```

Automating OpenNebula Deployment:

