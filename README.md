# rfe-ansible-edge-mgmt

Ansible automation managed through Ansible Tower to provision a RHEL for Edge Environments
## Solution Overview

This approach focuses on Ansible Tower to centrally manage the automation for edge resources. As new edge nodes are created, a [Provisioning Callback](https://docs.ansible.com/ansible-tower/latest/html/userguide/job_templates.html#provisioning-callbacks) triggers automation to be applied to edge node.

![Architecture](docs/images/architecture_overview.png)
## Prerequisites
### Download dependencies

Execute the following commands to download the required dependencies

```
$ ansible-galaxy role install -r collections/requirements.yaml
```

### Populate Inventory

Add the Image Builder instance(s) into the [hosts](inventory/hosts) file.

### Subscription Information

The content within this repository leverages Red Hat RPM's. The automation to manage subscribing the machines and repositories makes use of the [rhsm](https://github.com/redhat-cop/infra-ansible/blob/master/roles/rhsm) role within the [infra-ansible](https://github.com/redhat-cop/infra-ansible) repository.

It is recommended to add the necessary values to a separate file and inject it in as an extra parameter using `-e @<filename>` when executing the playbook. An example can be found below:

```
---
rhsm_username: "<password>"
rhsm_password: "<username>"
```

**Note:** If your machine is already subscribed, you can skip registration by passing `-e rhsm_manage=false`.

## Ansible Tower

Ansible automation is performed via Ansible Tower and Provisioning Callbacks. Perform the following configurations in an existing Ansible Tower environment:

### Credentials

Add a new [Machine Credential](https://docs.ansible.com/ansible-tower/latest/html/userguide/credentials.html) called **rfe** that will be used to connect to edge devices.

Enter the _username_ and _password_ of the edge device. By default, it is configured as _core_ and _edge_respectively. Be sure to also enter the password in the _Privilege Escalation Password_ field.
### Project

Add a new [Project](https://docs.ansible.com/ansible-tower/latest/html/userguide/projects.html) called **rfe-ansible-mgmt** that references the branch containing the automation.

Enter the SCM URL using the HTTPS clone address and the branch `ansible-edge`
### Inventory

Create a new [Inventory](https://docs.ansible.com/ansible-tower/latest/html/userguide/inventories.html) called **rfe** containing the following:

* Group called **localhost**
* Add the addresses of all edge devices as members to the of this group

### Job Template

Create a new Job template called **rfe** to execute the automation against the edge nodes:

* Associate the _rfe_ inventory
* Associate the _rfe-mgmt-ansible_. Select the **playbooks/rfe.yaml** playbook
* Associate te _rfe_ credential

Under _options_, select the following:

* Enable privilege escalation
* Enable provisioning callback

After selecting "Enable provisioning callback", generate a new Host config key. The Provisioning Callback URL and Host Config Key is required to be specified later.

## Provision

Execute the following command in order to provision the machine being sure to substitute the callback url and host config key in the extra variable parameters as shown below:

```
ansible-playbook -i inventory/ playbooks/image_builder.yaml -e '{"ansible_tower":{ "callback_url": "<callback_url>", "host_config_key":"<host_config_key>" }}'
```

Once provisioning is complete, a HTTPD container that exposes the RFE image and kickstart file on port 8000.

## Edge Nodes

Since the above automation exposes the kickstart file and the edge image, boot the RHEL edge instance and add the kickstart arguments as follows:

```
inst.ks=http://<image_builder_node>:8000/kickstart.ks
```

## Edge Automation

The key attribute of this architecture is that edge instances configure themselves by executing Ansible automation. This automation lives on a in the `ansible-edge` branch of this repository. A timer systemd service is configured to trigger the automation on a regular basis.
