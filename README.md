This Repo is for utilities/projects I manage on the Vagrant profile.

Unix Training Academy Lab Manager:

markdown
Copy code
# Unix Training Academy Lab Manager™ version 1.0-beta (Windows/PowerShell edition)

## README

This Vagrantfile provisions the Unix Training Academy training lab. The lab consists of a RHEL9 workstation and one or more other RHEL9 servers, named servera, serverb, etc., with a default setup of workstation + 2 servers, up to a maximum of 26 servers + workstation. The limitation is due to your system's memory capabilities. The workstation uses 2 GB of RAM, while the other servers use 1 GB RAM each. **Warning: exceeding 2 servers could deplete your system's memory.**

Three users `vagrant`, `student`, and `ansible` are defined in each VM (with initial passwords same as the username). By default, for user `vagrant`, Vagrant provides password-less key-based ssh login to each VM from Windows. This version is for Windows hosts only. The three users can use `sudo` within the VMs without a password.

Users `student` and `ansible` have password-based ssh login from the host OS (Windows) for each VM. In addition, `student` and `ansible` can ssh from any VM to any other VM without a password. Finally, for user `ansible`, the workstation also serves as an Ansible Control Node, while servers like servera, serverb, etc., serve as Ansible managed hosts with password-less ssh connection.

Ansible is installed on the Control Node (workstation), with a pre-loaded initial inventory file at `/etc/ansible/inventory` and config file at `/etc/ansible/ansible.cfg`. The ungrouped hosts in the inventory include the IP addresses, short names, and FQDN of all managed hosts. The Control Node (workstation) is accessible as localhost in Ansible. Only the FQDNs are used in the host groups in the inventory. This arrangement ensures that Ansible can connect to either the IPs, short names, or FQDNs without complaint. The default Ansible remote user is `ansible`. Privilege escalation to `root` is password-less and false by default.

### Required Environment variables

These can be permanently set in Windows or PowerShell before starting Vagrantfile, or set on the command line when starting Vagrantfile:

- `HOSTS` - the number of managed hosts; default 2 — **OPTIONAL; Default: 2**
- `REDHAT_USERNAME` - for Red Hat subscription manager — **MANDATORY**
- `REDHAT_PASSWORD` - same as above — **MANDATORY**

The password and username can also be input interactively and (optionally) saved permanently at runtime.

To permanently save any of these before starting Vagrantfile, in PowerShell, use the syntax below:

```powershell
[Environment]::SetEnvironmentVariable('REDHAT_USERNAME', 'example-jdoe1998', [EnvironmentVariableTarget]::User)
To delete a saved environment variable, in PowerShell, use the syntax below:

powershell
Copy code
[Environment]::SetEnvironmentVariable('REDHAT_USERNAME', $null, [EnvironmentVariableTarget]::User)
To specify an environment variable on the command line when starting Vagrant, use the syntax below:

powershell
Copy code
$env:HOSTS=2; $env:REDHAT_USERNAME="example-jdoe1998"; REDHAT_PASSWORD="example-k45U_T6hi0Yt" vagrant up
If The Lab Manager is unable to find the Red Hat username/password at runtime, the user will be prompted to specify them interactively, and can then optionally save them permanently to the OS.

To use the Vagrantfile, enter this command in PowerShell (start PowerShell as Administrator):

powershell
Copy code
$env:HOSTS=2; $env:REDHAT_USERNAME="example-jdoe1998"; REDHAT_PASSWORD="example-k45U_T6hi0Yt" vagrant up
Or simply: vagrant up (to use default values from the Environment) Then wait until it finishes provisioning, unless otherwise prompted for username/password. If HOSTS is not specified (command line or Environment) then a default value of 2 (for servera and serverb) will be used. (This default can be changed within the Vagrantfile, see below.)

Once the provisioning ends, from the Vagrantfile directory, ssh to workstation (starting with workstation is the typical use case, but servera, serverb, etc., is also possible):

bash
Copy code
vagrant ssh workstation
You will be automatically logged in as user vagrant (password not required). Once logged in, you can su to user student or ansible and can freely ssh from one server to another as user student or ansible. It is recommended to use user student for all routine tasks.

Alternatively, you can use another ssh client (like putty from the Windows host) to login as user vagrant (key-based) or user student or ansible (password-based). The ssh private key for user vagrant is located within your Vagrant project directory at .vagrant\machines\<server-name>\virtualbox\private_key. (Note: Only user vagrant has key-based ssh login from the Windows, using a client like putty.)

My recommended approach is to login directly as user student, and password student, using putty. Users student and ansible have password-based login from Windows host using putty (for example), and can do ssh password-less key-based connections to any of the provisioned servers once logged into any server.

To end a session without destroying the servers:

bash
Copy code
vagrant suspend # to save/suspend the VM, or
vagrant halt # to shut down
Warning: Do NOT run vagrant destroy unless you wish to destroy the VMs (e.g., at the end of a project/course after backing up your data).

To resume a suspended or halted session: vagrant up or vagrant resume

For help using vagrant: vagrant -h

This file must be saved with the name Vagrantfile in your project directory in Windows. Then download and install Oracle VirtualBox and Vagrant for Windows from their respective websites before running this Vagrantfile. It is not necessary to download a RHEL9 image first. It will be automatically downloaded the first time this Vagrant file is run on your system.

For updates, check out: Github: https://github.com/ecigwegbu/vagrant/uta-lab-manager/Vagrantfile

Author: Elias C. Igwegbu, B.ENG, MBA, MNSE, SWE-ALX/Holberton, RHCSA, AWS-CCP

Lab Manager™ © 2024 Unix Training Academy. All Rights Reserved.
