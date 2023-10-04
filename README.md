# avdsnapshot

/group_vars/otpjefflab.yml - configure the command list to execute

```
commands_list:
  - show hostname
  - show version
  - show management api http-commands
```

/getsnapshots.yml - configure the `hosts` parameter to select correct inventory of devices

```
- name: Collect Commands
  hosts: otpjefflab # represents inventory section to query
  connection: local # represents the local device will SSH to targets
```

# Report Generation


To remove the table of contents for each device:

Within File:
mrrobot/.ansible/collections/ansible_collections/arista/avd/roles/eos_snapshot/tasks/markdown.yml

Remove this stanza...

```
- name: Generate table of content report
  delegate_to: localhost
  ansible.builtin.template:
    src: md_table_of_content.j2
    dest: "{{ snapshots_backup_dir }}/{{ inventory_hostname }}/md_fragments/0_table_of_content.md"
    mode: 0664
```