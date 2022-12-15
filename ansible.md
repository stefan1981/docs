# Ansible

## General things
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

## Encryption
create an encrypted file
```
ansible-vault create encrypted-file.yml
```

edit encrypted files
```
ansible-vault edit encrypted-file.yml
```

encrypt an existing files
```
ansible-vault encrypt file.yml
```

decrypt a encrypted files
```
ansible-vault decrypt encrypted-file.yml
```

reset the password of an encrypted file
```
ansible-vault rekey encrypted-file.yml
```

create a encrypted file where the password comes from a file  
(the password.txt is a single-line file with a plain password in it)
```
ansible-vault create --vault-id label@password.txt encrypted-file.yml
```
