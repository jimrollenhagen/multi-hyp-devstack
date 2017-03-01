Ansible to set up a multi-node, multi-hypervisor devstack environment.

Sets up:

* a controller node without any compute hosts

* an ironic node with nova-compute, ironic services, and VMs that pretend to
  be bare metal.

* a virt node running nova-compute with the libvirt driver.

The point here is to be able to automate a deployment of this type so that
we can test behaviors in this and similar configurations.

Current status: WIP, moving quickly.
Goal status: works on newton and newer.

Usage
=====

# initial stack user setup via root access
ansible-playbook -i inventory.yml -u root stack-user.yaml

# do the rest
ansible-playbook -i inventory.yml -u stack main.yml
