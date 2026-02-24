1- vimrc file
2- dir create for inventory, roles and collections
=====================================================================================
3- create inventory file and configuration file
Create a static inventory file called /home/admin/ansible/inventory as follows:
-- node1.example.com is a member of the dev host group
-- node2.example.com is a member of the test host group
-- node3.example.com and node4.example.com are members of the prod host group
-- node5.example.com is a member of the balancers host group
-- The prod group is a member of the webservers host group
* Create a configuration file called ansible.cfg as follows
-- The host inventory file is /home/admin/ansible/inventory
-- The location of roles as /home/admin/ansible/roles
-- the location of collections is /home/admin/ansible/mycollections
-- remote user is root ( or as per question)
=====================================================================================
4-Configure the repository on all nodes.Create and playbook /home/admin/ansible/yum_repo.yml
The name of the repository is EX294_BASE
The description is "Ex294 Description"
The base URL is http://192.168.10.254/BaseOS
GPG signature checking is enabled
The GPG key URL is ftp://192.168.10.254/pub/rhel9.0/RPM-GPGKEY-redhat-release
The repository is enabled
-----------------
The name of the repository is EX294_APPSTREAM
The description is "Ex294 Description2"
The base URL is http://192.168.10.254/AppStream
GPG signature checking is enabled
The GPG key URL is ftp://192.168.10.254/pub/rhe9.0/RPM-GPG-KEY-redhat-release
The repository is enable
========================================================================================
5- Install ansible collections from url and place it under /home/admin/ansible/mycollections
url "https://galaxy.ansible.com/download/ansible-posix-2.0.0.tar.gz" 
url "https://galaxy.ansible.com/download/community-general-11.1.0.tar.gz"
======================================================================================
6- Install Packages Create a playbook called packages.yml that:
* Installs the php and mariadb packages on hosts in the dev, test, and prod host groups
* Installs the Development Tools package group on hosts in the dev host group.
* Updates all packages to the latest version on hosts in the dev host group
======================================================================================
7- Use System Roles   timesync or selinux
Install the RHEL system roles package and create a playbook called timesync.yml that,
-- Runs on all managed hosts
-- Uses the timesync role
-- Configures the role to use the time server 192.168.10.254
-- Configures the role to set the iburst parameter as enabled
-------------------------------
Create a playbook called selinux.yml and use system roles

 i) set selinux mode as enforcing on all managed nodes
 ===================================================================================
 8- Create and use a offline role
* Create a role called apache in /home/admin/ansible/roles with the following requirements
- - The httpd package is installed, enabled on boot, and started
- - The firewall is enabled and running with a rule to allow access to the web server
use A template file index.html.j2 is used to create the file /var/www/html/index.html with the following output: 
”Welcome to HOSTNAME on IPADDRESS”. where HOSTNAME is the fully qualified domain name
Create a playbook called httpd.yml that uses this role as follows: -- The playbook runs on hosts in the webservers host group
===================================================================================
9- Install roles using Ansible Galaxy.
-- Use Ansible Galaxy with a requirements file called /home/admin/ansible/roles/requirements.yml to download and install
roles to /home/admin/ansible/roles from the following URLs:
https://github.com/RedHatRanger/phpinfo.git                        (phpinfo)
https://github.com/RedHatRanger/balancer.git                       (balancer)
===============================================================================
10- Create a playbook called balance.yml as follows: The playbook contains a play that runs on hosts in the balancers host group and uses the balancer role.
This role configures a service to load balance web server requests between hosts in the webservers host group.
When implemented, browsing to hosts in the balancers host group (for example http://node5.example.com ) 
should produce the following output: Welcome to node3.example.com on 192.168.10.z
Reloading the browser should return output from the alternate web server:
Welcome to node4.example.com on 192.168.10.a
The playbook contains a play that runs on hosts in the webservers host group and uses the phpinfo role.
When implemented, browsing to hosts in the webservers host group with the URL /hello.php should produce the following output:
Hello PHP World from FQDN where FQDN is the fully qualified domain name of the host. 
For example, browsing to http://node3.example.com/hello.php, should produce the following output: Hello PHP World from node3.example.com
===============================================================================
11- Create a password vault
-- Create an Ansible vault to store user passwords as follows: The name of the vault is vault.yml
- The vault contains two variables as follows: dev_pass with value wakennym mgr_pass with value rocky The password to encrypt and decrypt the vault is atenorth
- The password is stored in the file /home/admin/ansible/password.txt
===================================================================================
12-Rekey an Ansible vault
Rekey an existing Ansible vault as follows:
Download the Ansible vault from http://192.168.10.254/ex407/secret.yml and save it as secret.yml
The current vault password is curabete The new vault password is newvare
The vault remains in an encrypted state with the new password
========================================================================================
13-Create a playbook cron.yml to set cron job user Natasha to execute the command “echo Ex294 exam” every 2 minutes on all nodes
=======================================================================================
14- Create a playbook called web as follows: 
The playbook runs on managed nodes in the dev host group 
Create the directory /webdev with the following requirements: - membership in the webdev group 
regular permissions: owner=read+write+execute, group=read+write+execute, other=read+execute ,
 special permissions: set group ID 
 Symbolically link /var/www/html/webdev to /webdev - 
 Create the file /webdev/index.html with a single line of text that reads: Development
 =======================================================================================
 15- Generate a hosts file
- Download an initial template file called hosts.j2 from http://192.168.10.254/ex407/ to /home/admin/ansible/
Complete the template so that it can be used to generate a file with a line for each inventory host in the same format as /etc/hosts
Create a playbook called gen_hosts.yml that uses this template to generate the file /etc/hosts on hosts in the dev host group.
When completed, the file /etc/hosts on hosts in the dev host group should have a line for each managed host:
127.0.0.1 localhost localhost.localdomain localhost4 localhost4.localdomain4 
::1 localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.10.x node1.example.com node1
192.168.10.y node2.example.com node2
192.168.10.z node3.example.com node3
192.168.10.a node4.example.com node4
192.168.10.b node5.example.com node5
======================================================================
16- Create a playbook called hwreport.yml that produces an output file called /root/hwreport.txt on all managed nodes with the following information:
-- Inventory host name
-- Total memory in MB
-- BIOS version
-- Size of disk device vda
-- Size of disk device vdb
-- Each line of the output file contains a single keyvalue pair.
Your playbook should:
-- Download the file hwreport.empty from the URL https://raw.githubusercontent.com/abhishek-tech-linux/RHCE9/refs/heads/main/rhce-practice-questions/golden_files/hwreport.txt
 and save it as /root/hwreport.txt
-- Modify with the correct values.
If a hardware item does not exist, the associated value should be set to NONE
Examplel file;->
#some hardware report info of system:
INVENTORY_HOSTNAME: hostname
BIOS_VERSION: biosversion
TOTAL_MEMORY: memory
VDA_SIZE: vdasize
VDB_SIZE: vdbsize
=============================================================================================
17-Modify file content
- Create a playbook called /home/admin/ansible/modify.yml as follows:
The playbook runs on all inventory hosts
The playbook replaces the contents of /etc/issue with a single line of text as follows
On hosts in the dev host group, the line reads: Development
On hosts in the test host group, the line reads: Test
On hosts in the prod host group, the line reads: Production
===========================================================================================
18-Create a playbook called motd.yml.

 i)  Run the playbook.
 ii) Whenever you ssh into any node (node1 here), the message will be as follows:
       Welcome to node1
       OS: RedHat 9.4
       Architecture: x86_64
======================================================================================================
19- Create user accounts
A list of users to be created can be found in the file called user_list.yml which you should download from
 https://raw.githubusercontent.com/abhishek-tech-linux/RHCE9/refs/heads/main/rhce-practice-questions/golden_files/user_list.yml
 and save to /home/admin/ansible/ Using the password vault created elsewhere in this exam, 
 create a playbook called create_user.yml that creates user accounts as follows:
Users with a job description of developer should be:
created on managed nodes in the dev and test host groups assigned the password from the dev_pass variable a member of supplementary group devops
Users with a job description of manager should be:
created on managed nodes in the prod host group assigned the password from the mgr_pass variable a member of supplementary group opsmgr
Passwords should use the SHA512 hash format.
Your playbook should work using the vault password file created elsewhere in this exam.
users:
- name: joe
job: manager
- name: bob:
job: developer
- name: alice
job: manager
================================================================================================
20- Create Logical volumes with ‘lvm.yml’ in all nodes according to following requirements.
* Create a new Logical volume named as 'data'
* LV should be the member of 'research' Volume Group
* LV size should be 1500M
* It should be formatted with ext4 file-system.
-If Volume Group does not exist then it should print the message "VG Not found"
-If the VG cannot accommodate 1500M size then it should print "LV cannot be created with following size", then the LV should be created with 800M of size.
-Do not perform any mounting for this LV.


lvname-data size-1500M  fs-ext4
vgname-research  if not exist message "VG Not found" 

=======================================================================================
21-Create and use partitions:

Create /home/x69van/ansible/partition.yml, which will create partitions on all the managed nodes: 

    a) After sdb creating a 1200M primary partition, partition number 1, and format it into ext4 filesystem.
    b) On the prod group to permanently mount the partition to /srv directory.
    c) If there is not enough disk space, give prompt information "Could not create partition of that size" and create a 800M partition.
    d) If sdb does not exist, a prompt message will be given "this disk does not exist."
