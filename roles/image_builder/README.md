Role Name
=========

Ansible role to build images for RHEL for EDGE

Requirements
------------

Requires podman installed (default on RFE), and internet connectivity to pull from quay.io and docker.io or configure a interally accessible repository to pull images from via the `ansible_runner` variable.

Role Variables
--------------

| Variable                       | Default                                   |
|--------------------------------|-------------------------------------------|
| rhsm_manage                    | true                                      |
| blueprints_tmp_directory       | /tmp/blueprints                           |
| container_image_directory_path | /tmp/container_build                      |
| yum_packages                   | see roles/image_builder/defaults/main.yml |
| systemd_services               | see roles/image_builder/defaults/main.yml |
| rfe_user                       | core                                      |
| rfe_password                   | edge                                      |
| rfe_git                        | see roles/image_builder/defaults/main.yml |
| rfe_automation_timer_sync      | 60min                                     |
| httpd_container_port           | 80                                        |
| httpd_host_port                | 8000                                      |
| ansible_runner                 | see roles/image_builder/defaults/main.yml |

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
---
- hosts: image_builder
  become: yes
  tasks:
    - name: Image Builder Role
      import_role:
        name: image_builder
```

License
-------

BSD

Author Information
------------------

Please contact via issues on github.
