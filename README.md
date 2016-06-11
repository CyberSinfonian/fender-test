# fender-test
Programming test for Fender Digital interview process

## System Requirements
- Python >= 2.6
- Ansible
- Boto

This code requires boto be installed in python. To install boto, please run 
```sh
$ pip install boto
```

# Setup
There are a few setup steps that must be taken to ensure proper functionality
of the system. Please ensure the steps below are followed thoroughly.

## System Variables
Some system variables need to be declared to ensure proper functionality of the system.
Primarily, the SSH host key checking should be disabled to prevent SSH timeout if
the user does not allow the key to be added to the known-hosts list promptly enough.
To do this, run the following command:

```sh
$ export ANSIBLE_HOST_KEY_CHECKING=False
```
This will disable the host key checking and will automatically add the new host keys
to the .ssh/known_hosts list.

Secondly, AWS access keys and secret keys need to be somehow passed into Ansible.
I have found that the easiest way to do this was via exporting the values as local
system variables:
```sh
$ export AWS_ACCESS_KEY_ID='AK123'
$ export AWS_SECRET_ACCESS_KEY='abc123'
```
It is also possible to keep the values in a vars_file:
```sh
---
ec2_access_key: "--REMOVED--"
ec2_secret_key: "--REMOVED--"
```
If going the route of the vars_file, please ensure to encrypt the keys with ansible-vault
for security purposes

## Dynamic Hosts Setup
Since this is a system for working with cloud instances, a dynamic hosts
list is more useful than a static one normally used with Ansible. The easiest
way to do this is with **ec2.py**, an external inventory script meant for 
working with EC2 environments. The easiest way to use the script is in-line
using Ansible's -i command line option after making the script executable. 

Alternatively, the file **ec2.py** can be copied to **/etc/ansible/hosts** and
made executable, and the file **ec2.ini** can be copied to 
**/etc/ansible/ec2.ini**, in which case Ansible can be run normally. However,
I personally have not tested this method as thoroughly, so I strongly
recommend using the command line option
