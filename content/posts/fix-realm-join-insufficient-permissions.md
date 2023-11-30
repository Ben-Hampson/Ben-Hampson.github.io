---
title: "Fix 'realm join' Insufficient Permissions"
date: 2023-11-30T18:05:35Z
draft: false
---
Recently I set up Ansible roles and playbooks to deploy some VMs. The role already existed to provision Ubuntu 18.04 VMs, so I was updating it to work with 22.04 VMs. One of the roles ran `realm join` to connect the VM to Active Directory. This command was running:

`realm join --user <realm-username> --computer-name <hostname> <realm domain name>`

Running this returned: `Insufficient privileges to modify computer account.` and `Insufficient privileges to join the domain.`

I knew this username and password was authorised to add computers to the AD because it was already doing it for 18.04 VMs. So why not for 22.04 VMs?

The error message is a red herring. After a couple of days of trying to find out the reason, I discovered it was because the AD didn't accept the computer name. The value of `--computer-name` must be 15 characters or less and be uppercase (I think). Anything other than this *exact name* would be rejected with this unhelpful error message. From my limited understanding, some AD's only accept this 'NetBIOS name' for your computer and no other value.

For fellow Ansible users, here are the tasks to create a variable called `computer_name` from the built-in Ansible variable `inventory_hostname` and pass that into the `realm join` command.

```yaml
- name: Set computer_name variable
  set_fact:
    computer_name: "{{ inventory_hostname | truncate(15, true, '', 0) | upper }}"

- name: realm join
  become: true
  ansible.builtin.command: "realm join --computer-name {{ computer_name }} {{ realm }}"
```