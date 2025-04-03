# ansible_aap_download  
=========  

This role downloads and extracts the **Ansible Automation Platform (AAP)** bundle, ensuring file integrity and authentication with the Red Hat portal.  

## Requirements  
------------  

- The system must have internet access to download the AAP bundle.  
- The user executing the playbook must have appropriate permissions to create directories and manipulate files.  
- A valid **offline_token** from the Red Hat portal is required.  
- The **SHA256** of the bundle (`aap_bundle_sha_value`) must be correctly provided.  

## Role Variables  
--------------  

The main variables used by the role are:  

- **`offline_token`**: Offline authentication token for accessing the Red Hat portal.  
- **`aap_bundle_sha_value`**: SHA256 hash of the AAP bundle for download validation.  
- **`user_info`**: List containing information about users responsible for storing the bundle.  
- **`bundle_file`**: Stores the results of the bundle verification on the system.  

## Dependencies  
------------  

No external dependencies from other roles in Ansible Galaxy.  

## Example Playbook  
----------------  

Example usage of the role:  

```yaml
---
- name: Download AAP
  hosts: automationcontroller
  gather_facts: true
  vars:
    user_info:
      - name: ansible-user
        home: /opt/aap

  tasks:
    - name: Import role
      ansible.builtin.import_role:
        name: laurobmb.ansible_aap_download.download
      vars:
        aap_bundle_sha_value: 88e40728986ecaa04cf3c2820ed0f4e5c85ae88c52353cc695dd5be9deab9725
        offline_token: 88e40728986ecaa04....
```  

## License  
-------  

BSD  

## Author Information  
------------------  

Created by Lauro Gomes, responsible for user configuration in the AAP environment.  