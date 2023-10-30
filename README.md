# opennebula-aoutomation
First step:

Remember: You must install the libvirt, ansible and git packages for the instalation

Version of packages:

libvirt 8
ansible 2.10


Create the template Download the ubuntu  22 for template configuration


inside of the util directory there are some files to help with:


netlab1.xml: to configure the invernal libvirt network, virsh net-import netlab1.xml.


virt_install.sh: the script to run and prepare the template vm, this step is interactive with the terminal to configure everything. e.g: name, ip, hostname…


Inside of inventory/host_vars/localhost.yml you must to setup some variables(it's look like this:)



![Uploading 2.JPG…]()




img_template: the template image name without extension template_address: the ip address set to the interface


for vm section you must inform ram, vcpu and net_type for networking selection(bridge or virtual network), and net_connector is the network name for virtual network or the bridge name for brige.




Deploying the vms
clone the repository and enter into the ubuntu directory
https://github.com/javad-nemati/opennebula-automation.git
cd create-vm-kvm/ubuntu
ansible-playbook -i inventory/hosts create-vm-kvm/ubuntu_vms-4nic.yml
you can destroy the vms with :

ansible-playbook -i inventory/hosts create-vm-kvm/destroy.yml



