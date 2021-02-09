# Manage Containers

[![GitHub Super-Linter](https://github.com/sabre1041/rfe-mgmt-ansible/workflows/Lint%20Code%20Base/badge.svg)](https://github.com/marketplace/actions/super-linter)

Manage Containers role for RHEL for Edge

## Requirements

Access to registry.access.redhat.com or a local registry with rhscl/httpd-24-rhel7:latest container in it

## Role Variables

| Variable        | Default                                                             |
|-----------------|---------------------------------------------------------------------|
| containers.name | httpd                                                               |
| containers.args | -p 8080:8080 registry.access.redhat.com/rhscl/httpd-24-rhel7:latest |
| ports           | 8080/tcp                                                            |

## Dependencies

RHEL for Edge

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
---
- hosts: localhost
  remote_user: root
  roles:
    - manage_containers
```

## License

BSD

## Author Information

Please contact via issues in github.
