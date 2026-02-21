Create and use partitions:

Create /home/x69van/ansible/partition.yml, which will create partitions on all the managed nodes: 

    a) After sdb creating a 1200M primary partition, partition number 1, and format it into ext4 filesystem.
    b) On the prod group to permanently mount the partition to /srv directory.
    c) If there is not enough disk space, give prompt information "Could not create partition of that size" and create a 800M partition.
    d) If sdb does not exist, a prompt message will be given "this disk does not exist."



































Create a playbook named partition.yml with the following content:

- name: Partition config
  hosts: all
  become: True
  tasks:

    - name: Checking details
      block:

        - name: Validate if sdb disk exists
          when: "'sdb' not in ansible_devices"
          ansible.builtin.debug:
            msg: this disk does not exist.

        - name: Create a new ext4 primary partition 1200MiB
          community.general.parted:
            device: /dev/sdb
            number: 1
            state: present
            fs_type: ext4
            part_end: 1200MiB
          when: "'sdb' in ansible_devices"
 
      rescue:
        - name: Cannot create of that size?
          when: "'sdb' in ansible_devices"
          ansible.builtin.debug:
            msg: Could not create partition of that size

        - name: Create a new ext4 primary partition 800MiB
          community.general.parted:
            device: /dev/sdb
            number: 1
            state: present
            fs_type: ext4
            part_end: 800MiB
          when: "'sdb' in ansible_devices"

      always:

        - name: Create a ext4 filesystem on /dev/sdb1
          when: "'sdb' in ansible_devices"
          community.general.filesystem:
            fstype: ext4
            dev: /dev/sdb1

        - name: Mounting ...
          when: inventory_hostname in groups['prod']
          ansible.posix.mount:
            path: /srv
            src: /dev/sdb1
            fstype: ext4
            state: mounted
Execute the playbook with the following command:

ansible-playbook partition.yml
To verify the partition creation, you can run the following command:

ansible all -a 'lsblk /dev/sdb' -b
