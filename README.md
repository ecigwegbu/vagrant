This Repo is for utilities/IaC projects I built on the Hashicorp's Vagrant platform.

# Unix Training Academy Lab Manager™ version 1.0-beta (Windows/PowerShell edition)
#
# README:
#
# This Vagrantfile provisions the Unix Training Academy training lab. The lab consists of a RHEL9 workstation
# and one or more other RHEL9 servers, named servera, serverb, etc (default: workstation + 2 others), up to 
# a maximum of 26 servers + workstation, limited by your system's memory. Workstation uses 2 GB of RAM while
# the other servers use 1 GB of RAM each. (Warning: exceeding 4 servers could run down your system's memory).
#
# Three users, vagrant, student, and ansible, are defined in each VM (with initial passwords same as username).
# By default, for user vagrant, Vagrant provides password-less key-based ssh login from Windows (this version 
# is for Windows host only) to each VM. The three users can use sudo within the VMs without password.
#
# Users student and ansible have password-based ssh login from the host OS (Windows), for each VM. In addition,
# student and ansible can ssh from any VM to any other VM without a password. 
# Finally, for user ansible, worstation also serves as an Ansible Control Node, while servers servera,
# serverb etc serve as Ansible managed hosts with password-less ssh connection.
#
# Ansible is installed on the Control Node (workstation), with pre-loaded initial inventory file at 
# /etc/ansible/inventory and config file at /etc/ansible/ansible.cfg. The ungrouped hosts in the inventory
# include the ip addresses, short names and fqdn of all managed hosts. The Control Node (workstation) is 
# accessible as localhost in Ansible. Only the fqdn are used in the host groups in the inventory file. 
# This arrangement ensures that Ansible can connect to either the ips, short names or fqdns without complaint.
# The default Ansible remote user is ansible. Privilege escalation to root is password-less,
# and set to false by default.
#
# To use the Vagrantfile (Lab Manager), first download and install Oracle VirtualBox and Vagrant from their respective 
# websites, and start VirtualBox. Then start PowerShell as Administrator and enter the command:
# vagrant up
# The Lab Manager will then begin to provision the workstation and (by default) two managed hosts - servera and serverb.
# To provision other than two managed hosts (e.g. 1, 3, 4 or more managed hosts), modify the command thus:
# $env:MANAGED_HOSTS=4; vagrant up                   [Note: $ is part of the command]
# The above example will then provision the workstation and 4 other managed hosts (servera, serverb, serverc
# and serverd). The system will record the specified number of hosts in its config file (./.vagrant/config.uta)
# for use the next time you run vagrant. You can manaully modify the config file, though this is not recommended.
# Warning: Your system could run out of memory if you try to provision too many servers. While the Lab Manager can
# handle up to 27 servers (workstation + 26 managed hosts), the practical limit will depend on the available RAM.
# Workstation uses 2GB RAM while the managed hosts use 1GB RAM each. For an 8GB system, for example, do not try to exceed
# 4 managed hosts).
# 
# The Lab Manager will also prompt you for the Red Hat Subscription Mangager username and password. This is required
# in order to install required software in the servers being provisioned. If you fail to provide these the program
# will abort. The username and password you provide interactively will be encripted and securely stored in your project
# directory for use the next time you run vagrant, such that the system will not prompt you again. However if
# you want to use a different set of credentials you can modify the vagrant command thus:
# $env:REDHAT_USERNAME='your-username'; $env:REDHAT_PASSWORD='your-redhat-password'; vagrant up
# (Of course you can add the MANAGED_HOSTS to the same command line as well if you wish). Credentials provided on the
# command-line override, and will then replace, the previous cached one for use next time you run vagrant.
# 
# The Lab Manager provides the following shared folder mapping between your Windows host and the workstation VM:
# Windows folder: ./.vagrant/workstation_shared    (this is located in your project directory)
# Red Hat Linux server 'workstation' directory: /home/student/windows_shared
# Files saved in either directory can be read from the other by the student user.
# Note: Your project directory is any directory you choose yourself for your project data; 
# Vagrantfile must be located within it. If the shared folder does not exist, the Lab Manager will create it on start-up.
# 
# The first time you run Vagrantfile, it may automatically download the box image from its online repository,
# so the process may take longer than usual. This image is Vagrant-specific box image for the VirtualBox
# provider (e.g. 'generic/rhel9' available from Vagrant Hub). (Note: A Red Hat .ISO image is not required.)
#
# Once the provisioning ends, you can use an SSH client like Putty to connect from Windows to any of the  
# servers, and as any of the three users - student, ansible or vagrant. The server's ip address are:
# 192.168.56.10 (workstation), 192.168.56.11 (servera), 192.168.56.11 (serverb) etc. In addition, 
# user vagrant can also connect password-less from PowerShell using the pem private key stored at:
# '.vagrant\machines\<server-name>\virtualbox\private_key'. (Note: Only user vagrant has key-based
# SSH login from the Windows host.) 
# 
# You can also connect to any of the servers using the default vagrant account from PowerShell:
# vagrant ssh workstation
# You will be automatically logged in as user vagrant (password not required).
# Once logged in, you can su to user student or ansible and can then freely ssh from one server to another 
# as user student or ansible. It is recommended to use user student for all routine tasks.
#
# My recommended approach is to login directly as user student, and password student, using Putty.
# Users student and ansible have password-based login from the Windows host using Putty (for example),
# and can do ssh password-less key-based connections from any VM to any of the provisioned VMs once logged into
# any VM.
#
# To end a session without destroying the servers:
# vagrant suspend (to save/suspend the VM), or
# vagrant halt (to power off the VM)
# Warning: Do NOT run 'vagrant destroy' unless you wish to destroy the VMs (eg at the end 
# of a project/course after backing up your data)
#
# To resume a suspended VM: vagrant resume
# To restart a halted VM: vagrant up
# To halt and restart a VM: vagrant reload
# For help using vagrant: vagrant -h
# 
# For updates, checkout: GitHub: https://github.com/ecigwegbu/vagrant/blob/main/uta-lab-manager/Vagrantfile
#
# Author: Elias C. Igwegbu, B.ENG, MBA, MNSE, SWE-ALX/Holberton, RHCSA, AWS-CCP
# Lab Manager™ © 2024 Unix Training Academy. All Rights Reserved.
