# avdsnapshot - Generate single report for all device CLI output

This Ansible playbook uses the Arista Validated Designs AVD reference and EOS Snapshot role to generate a single report output. 
Reference these documentation locations for more information:
- https://avd.sh/en/stable/
- https://avd.sh/en/stable/roles/eos_snapshot/index.html

# Configuration
/group_vars/aristainfra.yml - configure the command list to execute against the devices defined in group "[aristainfra]"

```
commands_list:
  - show hostname
  - show management api http-commands
```

/getsnapshots.yml - configure the `hosts` parameter to select correct inventory of devices

```
- name: Collect Commands
  hosts: aristainfra # represents inventory section to gather EOS Snapshots from.
  connection: local # represents the local device will SSH to targets
```

# Report Generation

To remove the table of contents generated for each device, customize the AVD defaults as shown below.

Within File:
`<user>/.ansible/collections/ansible_collections/arista/avd/roles/eos_snapshot/tasks/markdown.yml`

Remove this stanza:

```
- name: Generate table of content report
  delegate_to: localhost
  ansible.builtin.template:
    src: md_table_of_content.j2
    dest: "{{ snapshots_backup_dir }}/{{ inventory_hostname }}/md_fragments/0_table_of_content.md"
    mode: 0664
```

# Output
A markdown file concatenating all of the individual device outputs is saved to `All-Device-Report.md`
The individual snapshots are saved locally within `./snapshots/`