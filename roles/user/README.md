# ansible_aap_user_setup  
=========  

This role creates and configures users on the system for the installation and operation of the **Ansible Automation Platform (AAP)**, including SSH key creation and sudo permissions.  

## Requirements  
------------  

- Ansible must have permissions to create users and modify system configuration files.  
- The `community.crypto` module must be available for OpenSSH key generation.  
- The `automationcontroller` group must be defined in the inventory to enable the generation and distribution of SSH keys.  

## Role Variables  
--------------  

The main variables used by the role are:  

- **`user_info`**: List of dictionaries containing:  
  - `name`: Name of the user to be created.  
  - `password`: User password.  
  - `home`: User's home directory.  
  - `key`: Public SSH key for authorization.  

- **`sshkey.results`**: Stores information about the generated SSH key.  

## Dependencies  
------------  

No external dependencies from other roles in Ansible Galaxy.  

## Example Playbook  
----------------  

Example usage of the role:  

```yaml
---
- name: Configure Ansible User
  hosts: automationcontroller
  gather_facts: true
  vars:
    user_info:
      - name: ansible-user
        password: <your openssl hash>
        key: 'ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICGzwrDAfrzylGJsj9i9bcNnduC34FaLLJZd3uzMk/fV user@hulk'
        home: /opt/aap

  tasks:
    - name: Import role
      ansible.builtin.import_role:
        name: laurobmb.ansible_aap_download.user

```  

## License  
-------  

BSD  

## Author Information  
------------------  

Created by Lauro Gomes, responsible for user configuration in the AAP environment.  