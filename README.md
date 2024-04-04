This Repo is for utilities/IaC projects I built on the Hashicorp's Vagrant platform.

# Unix Training Academy Lab Manager™ version 1.0-beta (Windows/PowerShell edition)

## README

This Vagrantfile provisions the Unix Training Academy training lab. The lab consists of a RHEL9 workstation and one or more other RHEL9 servers, named servera, serverb, etc., with a default setup of workstation + 2 servers, up to a maximum of 26 servers + workstation, limited by your system's memory. The workstation uses 2 GB of RAM, while the other servers use 1 GB RAM each. **Warning: exceeding 2 servers could deplete your system's memory.**

Three users `vagrant`, `student`, and `ansible` are defined in each VM (with initial passwords the same as the username). By default, for user `vagrant`, Vagrant provides password-less key-based SSH login to each VM, from Windows. This version is for Windows host only. The three users can use `sudo` within the VMs without a password.

Users `student` and `ansible` have password-based SSH login from the host OS (Windows), for each VM. In addition, `student` and `ansible` can SSH from any VM to any other VM without a password. Finally, for user `ansible`, workstation also serves as an Ansible Control Node, while servers servera, serverb, etc., serve as Ansible managed hosts with password-less SSH connection.

Ansible is installed on the Control Node (workstation), with a pre-loaded initial inventory file at `/etc/ansible/inventory` and a config file at `/etc/ansible/ansible.cfg`. The ungrouped hosts in the inventory include the IP addresses, short names, and FQDN of all managed hosts. The Control Node (workstation) is accessible as `localhost` in Ansible. Only the FQDNs are used in the host groups in the inventory. This arrangement ensures that Ansible can connect to either the IPs, short names, or FQDNs without complaint. The default Ansible remote user is `ansible`. Privilege escalation to root is password-less, and false by default.

### Required Environment Variables

They can be **PERMANENTLY** set in Windows or PowerShell, before starting Vagrantfile, or **TEMPORARILY** set on the command line when starting Vagrantfile:

- **HOSTS** - the number of managed hosts; default 2         ---OPTIONAL; Default: 2---
- **REDHAT_USERNAME** - for Red Hat subscription manager     ---MANDATORY---
- **REDHAT_PASSWORD** - ==do==                               ---MANDATORY---

The password and username can also be input interactively at runtime, but for **TEMPORARY** use only.

To **PERMANENTLY** save any of these before starting Vagrantfile, in PowerShell, use the syntax below, and then restart PowerShell **BEFORE** running the vagrant command:

```powershell
[Environment]::SetEnvironmentVariable('REDHAT_USERNAME', 'example-jdoe1998', [EnvironmentVariableTarget]::User)
To delete a saved environment variable, in PowerShell, use the syntax below:

powershell
[Environment]::SetEnvironmentVariable('REDHAT_USERNAME', $null, [EnvironmentVariableTarget]::User)
IMPORTANT: You must restart PowerShell for these changes to take effect, BEFORE running vagrant up (see below).

To specify an environment variable TEMPORARILY on the command line when starting Vagrant, use the syntax below, but note that the variable will be requested again next time you try to run vagrant:

powershell
$env:HOSTS=2; $env:REDHAT_USERNAME="example-jdoe1998"; REDHAT_PASSWORD="example-k45U_T6hi0Yt"; vagrant up
If The Lab Manager is unable to find the Red Hat username/password at runtime, the user will be prompted to specify them interactively but this will be for TEMPORARY use only.

To use the Vagrantfile, enter this command in PowerShell (start PowerShell as Administrator):

powershell
$env:HOSTS=2; $env:REDHAT_USERNAME="example-jdoe1998"; REDHAT_PASSWORD="example-k45U_T6hi0Yt"; vagrant up
Or simply: vagrant up (to use saved/default values from the Environment) Then wait until it finishes provisioning the VMs, unless otherwise prompted for username/password. If HOSTS is not specified (command line or Environment) then a default value of 2 (for servera and serverb) will be used. (This default can be changed within the Vagrantfile, see below).

Once the provisioning ends, you can use a client like PuTTY to connect from Windows to any of the three users - student, ansible, or vagrant - on any of the servers, using their password. The IP addresses are: 192.168.56.10 (workstation), 192.168.56.11 (servera), 192.168.56.12 (serverb), etc. In addition, user vagrant can also connect from PuTTY password-less using the private key stored at .vagrant\machines\<server-name>\virtualbox\private_key. (Note: Only user vagrant has key-based SSH login from Windows, using a client like PuTTY).

Alternatively, once the provisioning ends, from the Vagrantfile directory, SSH to workstation (starting with workstation is the typical use case, but servera, serverb, etc., is also possible) thus:

bash
vagrant ssh workstation
You will be automatically logged in as user vagrant (password not required). Once logged in, you can su to user student or ansible and can freely SSH from one server to another as user student or ansible. It is recommended to use user student for all routine tasks.

My recommended approach is to log in directly as user student, and password student, using PuTTY. Users student and ansible have password-based login from the Windows host using PuTTY (for example), and can do SSH password-less key-based connections from any VM to any of the provisioned VMs once logged into any VM.

To end a session without destroying the servers:

bash
vagrant suspend # to save/suspend the VM, or
vagrant halt # to power off the VM
Warning: Do NOT run vagrant destroy unless you wish to destroy the VMs (e.g., at the end of a project/course after backing up your data).

To resume a suspended VM: vagrant resume
To restart a halted VM: vagrant up
For help using vagrant: vagrant -h

This file must be saved with the name Vagrantfile in your project directory in Windows. Then download and install Oracle VirtualBox and Vagrant for Windows from their respective websites before running this Vagrantfile. It is not necessary to download a RHEL9 image first. It will be automatically downloaded the first time this Vagrant file is run on your system.

For updates, check out: Github: https://github.com/ecigwegbu/vagrant/uta-lab-manager/Vagrantfile

Author: Elias C. Igwegbu, B.ENG, MBA, MNSE, SWE-ALX/Holberton, RHCSA, AWS-CCP

Lab Manager™ © 2024 Unix Training Academy. All Rights Reserved.
