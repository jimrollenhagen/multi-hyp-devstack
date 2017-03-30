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

Assumes an account on Rackspace public cloud, with credentials configured
in a clouds.yaml file *and* a pyrax configuration file. The OpenStack modules
don't support Rackspace's DNS (because it isn't OpenStack), so we rely on the
pyrax-based rax modules there. Also assumes a domain delegated to Rackspace's
DNS servers, and either not associated with a Rackspace cloud account, or
already associated with the cloud account configured for pyrax.

Usage
=====

Copy `secrets.yml.example` to `secrets.yml` and edit as necessary.

Install the items in `requirements.txt`::

  pip install --upgrade -r requirements.txt

Build the machines::

  ansible-playbook -i inventory.yml build.yml

Destroy machines and private network::

  ansible-playbook -i inventory.yml destroy.yml

Initial stack user setup via root access::

  ansible-playbook -i inventory.yml -u root stack-user.yaml

Run devstack to do all the things::

  ansible-playbook -i inventory.yml -u stack main.yml

TODO
====

* Disable root SSH ASAP
* Change root password ASAP
* Support private networks (and don't listen on non-private networks)
  * will need something on publicnet for UI/API
* Don't expose my inventory file, I guess
* move SWIFT_ENABLE_TEMPURLS=True and SWIFT_TEMPURL_KEY=secretkey to controller
* ironic subnodes don't do everything I need
  * create flavor
  * create/upload images
* ironic plugin is probably totally fucked
  * ya, rework this to check if resources exist, not if subnode
