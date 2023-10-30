
# opennebula-automation
First step:

Remember: You must install the libvirt, ansible and git packages for the instalation

Version of packages:

libvirt 8
ansible 2.10


Create the template Download the ubuntu  22 for template configuration


inside of the util directory there are some files to help with:

netlab1.xml: to configure the invernal libvirt network

```
virsh net-import netlab1.xml
```


```
bash virt_install.sh
```

in this example, we have four network interfaces (NICs) and are creating two bonds using them
Creating a template is entirely up to you, but let's assume that the switch ports above the machines are tagged with VLAN 180 and VLAN 20
the script to run and prepare the template vm, this step is interactive with the terminal to configure everything. e.g: name, ip, hostname…


Inside the inventory/host_vars/localhost.yml file, you need to set up some variables. These variables might look like this:

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
https://github.com/javad-nemati/opennebula-automation.git
```
```
cd create-vm-kvm/ubuntu
ansible-playbook -i inventory/hosts create-vm-kvm/ubuntu_vms-4nic.yml
```
you can destroy the vms with :
```
ansible-playbook -i inventory/hosts create-vm-kvm/destroy.yml
```




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

Single Front-end with Local Storage: This scenario involves a single front-end hosting all the OpenNebula services and a set of hosts that act as hypervisors to run Virtual Machines (VMs). The virtual disk images are stored in local storage, with the front end hosting an image repository (image datastore). These images are subsequently transferred from the front end to the hypervisors to initiate the VMs.
How to Use the Playbooks

Once we have the repository downloaded, make sure to download the necessary dependencies by running the following command inside the repository path:
```
ansible-galaxy collection install -r requirements.yml
```
will use the inventory of a Single Front-end with Local Storage that we referenced in the previous section:

```
---
all:
  vars:
    ansible_user: root
    one_version: '6.6'
    one_pass: opennebulapass
    ds:
      mode: ssh
    vn:
      admin_net:
        managed: true
        template:
          VN_MAD: bridge
          PHYDEV: eth0
          BRIDGE: br0
          AR:
            TYPE: IP4
            IP: 192.168.200.100
            SIZE: 48
          NETWORK_ADDRESS: 192.168.200.0
          NETWORK_MASK: 255.255.255.0
          GATEWAY: 192.168.200.1
          DNS: 1.1.1.1

frontend:
  hosts:
    fe1: { ansible_host: 192.168.200.14 }

node:
  hosts:
    node1: { ansible_host: 192.168.200.10 }
    node2: { ansible_host: 192.168.200.12 }

```

To use one deploy with the above inventory, there are two possible ways:

Directly using the one-deploy playbooks with the following command in the root path of the repository:
```
ansible-playbook -i inventory/example.yml opennebula.deploy.main
```

