# Ansible

verify all hosts in your inventory (/etc/ansible/hosts)
```
ansible all --list-hosts
```

ping the existing hosts 
```
ansible all -m ping
```

create an inventory file (inventory.yaml)
```
virtualmachines:
  hosts:
    vm01:
      ansible_host: ip_of_machine_to_manage
    vm02:
      ansible_host: ip_of_machine_to_manage
```

verify your inventory file
```
ansible-inventory -i inventory.yaml --list
```

ping the machines in the inventory
```
ansible virtualmachines -m ping -i inventory.yaml
```
