# Ansible Role: ubnt-bootstrap 
---------------------

This ansible role will bootstrap SSH keys using a ssh connection to your UBNT Unifi and Edgerouter Devices.


## Role Variables:
---------------------

The following role variables work for EdgeRouterX devices.

```
ubnt_ssh_authorized_key: ~/.ssh/id_ed25519.pub
ubnt_device_user_id: ubnt
ansible_network_os: edgeos
ansible_user: ubnt
ansible_pass: ubnt
ansible_ssh_private_key_file: /etc/ansible/keys/id_rsa
ansible_net_ssh__key_file: /etc/ansible/keys/id_rsa
ansible_python_interpreter: /usr/bin/python
```

The following role variables work for UnFi USG and USG4P  devices.
```

## Installation
---------------------

ansible-galaxy install ppouliot.ubnt-bootstrap


## Bootstrap Playbook
---------------------

Now you can simply add the following to your playbook file and include it in your site.yml so that it runs on all hosts in the ubnt group.

```
- name: UniFi USG Bootstrap SSHKeys for Ansible
  hosts: edgerouterx-by-ip
  connection: ssh
  become: yes
  become_user: root
  gather_facts: yes
  tasks:
    - debug: var=ansible_connection
  roles:
    - ppouliot.ubnt_bootstrap
```

Make sure that gather_facts is set to false, otherwise ansible will try to first gather system facts using python which is not yet installed!

## Contributors
---------------------

 * Peter Pouliot <peter@pouliot.net>

## Copyright and License
---------------------

Copyright (C) 2018 Peter J. Pouliot

Peter Pouliot can be contacted at: peter@pouliot.net

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

