# Ansible PanOs

This script helps engineers to automate PanOs firewalls configuration through Panorama.


### Requirements

This script tested on Linux/WSL with python3.9.  
The following packages are required:
 - python3.9
 - ansible
 - pan-os-python
 - pan-python
 - ansible-galaxy collection paloaltonetworks.panos

It's recommended to crate a virtual environment, activate it and then install the packages:

```sh
$ sudo apt install python3.9 python3.9-venv python3-pip
$ git clone https://github.com/Pr1meSuspec7/ansible-panos.git
$ cd ansible-panos
$ python3.9 -m venv VENV-NAME
$ source VENV-NAME/bin/activate
$ pip install ansible
$ pip install -r requirements.txt
$ ansible-galaxy collection install paloaltonetworks.panos
```
>NOTE: chose a name for virtual environment and replace the `VENV-NAME` string


### How it works

The playbook `playbook-panorama-create-rules.yml` creates object and security rules.  
The playbook `playbook-panorama-delete-rules.yml` delete object and security rules.  
The playbook `playbook-panorama-commit-rules.yml` commit and push object and security rules.  
To create new objects and rules just fill in the `data.yml` file.  
>NOTE: You should NOT edit playbooks.

### How to use

Add ip address, username and password of your Panorama in the `host_vars/panorama.yml` file:
```yaml
---
ip_address: "10.10.10.10"
username: "admin"
password: "supersecretpassword"
```

Fill in the `data.yml` file (look at the file `data_sample.yml` as an example).  
In the square brackets you can put multiple fields separated by the comma because they are lists:
```yaml
security_rules:
  - device_group: pan-lab
    rule_name: cloudflare_dns
    src_zone: ["CC-VLAN", "SEC-84"]
    src_ip: ["any"]
    dst_zone: ["untrust"]
    dst_ip: ["cloudflare_dns"]
    app: ["dns"]
    service: ["application-default"]
    action: allow
```

Each section corresponds to a task, if an object or rule does not need to be configured 
simply delete or comment out the correct section:
```yaml
#address_object:
#  - device_group: pan-lab
#    object_name: cloudflare_dns
#    object_type: ip-netmask
#    object_value: "1.1.1.1"
```

To perform a check without modification run:
```sh
$ ansible-playbook playbook-panorama-create-rules.yml --check
```

To perform a configuration change run:
```sh
$ ansible-playbook playbook-panorama-create-rules.yml
```

To push configuration on managed firewalls:
```sh
$ ansible-playbook playbook-panorama-commit-push.yml
```

# References
[Ansible for PAN-OS](https://pan.dev/ansible/docs/panos/)  
[Ansible-galaxy Collection paloaltonetworks.panos](https://galaxy.ansible.com/ui/repo/published/paloaltonetworks/panos/)

# MIT License
Feel free to edit, improve and share.
