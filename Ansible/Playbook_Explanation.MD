### Below is a Sample Ansible Playbook, with basic explanation of the terms used in a playbook 

```sh
---
- name: playbook to create directory
  hosts: webserver
  user: mynew
  become: true
  become_user: root
  become_method: sudo
  tasks:
  - name: task_1
  - name: task_2
  
```
> **name**: A user-friendly name for the playbook.
> **hosts**: Specifies the target host or group of hosts (in this case, "webserver").
> **user**: Specifies the remote user to use when connecting to the host.
> **become**: Indicates that privilege escalation is required.
> **become_user**: Specifies the user to become (in this case, "root").
> **become_method**: Specifies the method for privilege escalation (in this case, "sudo").
> **tasks**: Defines a list of tasks to execute.
