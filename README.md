# elasticsearch-test
Programming test for previous interview process.
The work here is in a partially-functional state after running out of time to complete the programming test.

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

## Variable Configuration
The variables used in the creation of the EC2 instances and in the execution of the
installation and configuration roles can be found in the **hosts** file.  The defaults
are set to use a basic Amazon Ubuntu 14.04 server AMI, a t2.micro instance type, the
us-west-2 Oregon region, the subnet group us-west-2a, and assign the security group
'elasticsearch'.  There is also a username field that is used with the access and
installation of the new remote server instances.  This should only be changed if
the AMI is changed to a different system that requires a different username for 
remote access.  Otherwise, these basic settings should be used as the default.

The servers are created at two points.  One server is created first as the master server,
and is given a tag specifying that it is a master, then two slave servers are created with
the appropriate tag.

## Execution
In order for execution to be successful, the values for AWS_ACCESS_KEY_ID and
AWS_SECRET_ACCESS_KEY must be set, using either of the methods mentioned above.
I have thoroughly tested the export of those values as system variables, so that is
the method I recommend for script execution.  Here is the command to run to launch the
instances:
```ssh
$ ansible-playbook -v -i hosts launch-elasticsearch-ec2.yml
```
I find using the -v flag for minimum verbosity to be useful for general execution
purposes to track how the systems are working and to confirm that all of the systems
are coming up properly.  Optionally, increasing the verbosity up to as high as -vvvv
greatly increases the amount of logging and visibility into the scripts and system
creation.
