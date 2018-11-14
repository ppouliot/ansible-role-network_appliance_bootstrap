# Ansible Role: ubnt_device_bootstrap 
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
ansible_pass: <YOUR_EDGEROUTER_PASSWORD>
ansible_ssh_private_key_file: /etc/ansible/keys/id_rsa
ansible_net_ssh__key_file: /etc/ansible/keys/id_rsa
ansible_python_interpreter: /usr/bin/python
```

The following role variables work for UnFi USG and USG4P devices.

```
ubnt_ssh_authorized_key: ~/.ssh/id_ed25519.pub
ubnt_device_user_id: admin
ansible_network_os: edgeos
ansible_ssh_user: admin
ansible_user: admin
ansible_ssh_pass: <YOUR_USG_PASSWORD>
ansible_pass: <YOURT_USG_PASSWORD>
become: yes
ansible_ssh_private_key_file: /etc/ansible/keys/id_rsa
ansible_net_ssh__key_file: /etc/ansible/keys/id_rsa
ansible_python_interpreter: /usr/bin/python
```

## Installation:
----------------

```
ansible-galaxy install ppouliot.ubnt_device_bootstrap
```

## Example Inventory:
---------------------

```
localhost ansible_connection=local ansible_python_interpreter="/usr/bin/env python"
  
[usg]
usg4p.pouliot.net

[usg-by-ip]
192.168.1.1

[edgerouterx]
erx.pouliot.net

[edgerouterx-by-ip]
192.168.1.2

[cloudkey]
Unifi-Cloudkey.pouliot.net

[cloudkey-by-ip]
192.168.1.3

[ssh_connection]
pipelining=True

```


## Bootstrap Playbook:
----------------------

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

## Example Playbook
-------------------

```

#!/usr/bin/env ansible-playbook
---

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

- hosts: edgerouterx
  connection: network_cli
  gather_facts: false
  tasks:
  - name: Collect facts from EdgeOS Devices
    edgeos_facts:
      gather_subset: all

- name: UniFi USG Bootstrap SSHKeys for Ansible
  hosts: usg-by-ip
  connection: ssh
  become: yes
  become_user: root
  gather_facts: false
  tasks:
    - debug: var=ansible_connection
  roles:
    - ppouliot.ubnt_bootstrap

- hosts: usg
  connection: network_cli
  gather_facts: false
  tasks:
  - name: Collect facts from Unifi Devices
    edgeos_facts:
      gather_subset: all

```


## Contributors:
---------------------

 * Peter Pouliot <peter@pouliot.net>

## Copyright and License:
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

