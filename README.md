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

Copy `default.yml.template` to `default.yml`

!! WARNING: BE SURE TO EDIT THIS, namely, the key_pair_name var !!
```
cp group_vars/default.yml.template group_vars/default.yml
```

## AWS

### Prerequisites

#### Encrypted Vault File

We will make use of the ansible vault to store access and secret access keys
as they are generated on AWS. To begin, create a password file and place it in secure
location. For example `~/.ansible/vault-password-file`. DO NOT COMMIT THIS FILE TO THE
REPO OR YOU WILL NEED TO ROTATE KEYS. This is just plain text file which contains single
string password, e.g. 'password'.

The credentials are stored as files and encrypted/decrypted. We store these in `group_vars/credentials.yml`. While the file is encrypted, to be safe it is also in `~/.gitignore`.

To view the valuables for debugging purposes, you can decrypt it with ansible-vault:

```
ansible-vault decrypt --vault-password-file ~/.ansible/vault_password_file group_vars/credentials.yml
```

The file will be unencrypted on disk. To re-encrypt, use the following command:

```
ansible-vault encrypt --vault-password-file ~/.ansible/vault_password_file group_vars/credentials.yml
```

#### AWS Access Keys

Log in as the Root User:

Go to the AWS Management Console at https://aws.amazon.com/console/.
Log in using the root user email and password.

Access Security Credentials:

Click on your account name at the top right of the AWS Management Console.
Click `Security credentials` section.
Click `Create Access Keys`
Download the `rootkey.csv`
Place this in a yaml file (e.g. `group_vars/credentials.yml` ) with the following format:

aws:
  root:
    access_key: YOUR_ACCESS_KEY
    secret_access_key: YOUR_SECRET_ACCESS_KEY

Encrypt this file:
```
ansible-vault encrypt --vault-password-file ~/.ansible/vault_password_file group_vars/credentials.yml
```

### Image ID

To find the latest AMI ID, go to https://us-east-2.console.aws.amazon.com/ec2/home?region=us-east-2#AMICatalog

Search for "DLAMI" for machine learning instances.

### Launch an EC2 instance

```
ansible-playbook --vault-password-file ~/.ansible/vault_password_file -e "credentials_yaml=group_vars/credentials.yml" launch_ec2.yml
```

Optionally, you can define new instance_name and instance_type over the default ones provided in `group_vars/default.yml`

```
ansible-playbook --vault-password-file ~/.ansible/vault_password_file -e "credentials_yaml=group_vars/credentials.yml" -e "instance_name_prefix=webservice" -e "instance_type=t2.small" launch_ec2.yml
```

### Terminate an EC2 instance

```
ansible-playbook --vault-password-file ~/.ansible/vault_password_file -e "credentials_yaml=group_vars/credentials.yml" terminate_ec2.yml
```
