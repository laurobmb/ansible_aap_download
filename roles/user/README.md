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
- hosts: servers
  roles:
    - { role: ansible_aap_user_setup, user_info: 
        [
          { name: "admin", password: "password_hash", home: "/home/admin", key: "ssh-rsa AAAAB3..." }
        ] 
      }
```  

## License  
-------  

BSD  

## Author Information  
------------------  

Created by Lauro Gomes, responsible for user configuration in the AAP environment.  