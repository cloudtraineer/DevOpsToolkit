#Below is a Sample Ansible Playbook, with basic explanation of the terms used in a playbook 

```sh
---
- name: playbook to create directory
  hosts: webserver
  user: mynew
  become: true
  become_user: root
  become_method: sudo
  tasks:
  - name: creating a directory
    file:
      path: /path/to/directory  # Specify the directory path you want to create
      state: directory
      mode: 0777  # You can set the appropriate permissions
      owner: ec2-user
```
