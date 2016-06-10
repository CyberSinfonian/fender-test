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
