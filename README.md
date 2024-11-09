# Borden Ansible
Infrastructure utilities for my services

## Layout
https://docs.ansible.com/ansible/latest/tips_tricks/sample_setup.html

## Prerequisites

1. Ansible installed
```
brew install ansible
```

2. Install ansible requirements:

```
ansible-galaxy install -r requirements.yml
```

3. Setup `group_vars/default.yml`

Copy the vars.yml.template to vars.yml

!! WARNING: BE SURE TO EDIT THIS, namely, the key_pair_name var !!
```
cp group_vars/default.yml.template group_vars/default.yml
```
