# rfe-ansible-edge-mgmt

Approaches for using Ansible to manage the lifecycle of RHEL for Edge (RFE) environments.

## Overview

This repository contains several approaches demonstrating how Ansible automation can be used to manage a RHEL for Edge environment. Each scenarios is separated onto its own branch and reference a common branch containing the Ansible automation. 

The following scenarios are available:

1. `ansible-engine` - Ansible automation executed directly on the RFE node
2. `ansible-runner` - Ansible automation executed within a container running on the RFE node
3. `ansible-tower` - Ansible automation driven using Ansible Tower. Provisioning Callbacks are used to trigger the automation when the RFE node starts.

More information can be found in the respective branches.

