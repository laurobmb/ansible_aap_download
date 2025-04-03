# Ansible Collection - laurobmb.ansible_aap_download

The playbooks are designed to **create and configure a user for installing Ansible Automation Platform (AAP)** and **download and extract the AAP installation package**, ensuring that the environment is properly prepared for deployment.

## 1. Creating and Configuring the Ansible User

The first set of tasks, imported via the role `laurobmb.ansible_aap_download.user`, creates a specific system user (`ansible-user`), sets its password, and configures SSH access. Additionally, it adjusts sudo permissions and generates an SSH key for secure authentication.

To set the password for `ansible-user`, use the following command to generate an encrypted password:

```bash
openssl passwd -1 <your password>
```

The generated value should be used in the `user_info.password` variable within the playbook.

## 2. Downloading and Extracting the AAP Package

The second set of tasks, imported via the role `laurobmb.ansible_aap_download.download`, authenticates with the Red Hat portal using an **offline token** and checks if the AAP package file already exists on the server. If not, the playbook downloads it directly from the Red Hat portal, validates its checksum to ensure integrity, and finally extracts the content to the appropriate directory.

### How to Obtain Authentication Tokens for the Red Hat API

To automate the download of Ansible Automation Platform (AAP) using the Red Hat API, you need to obtain an **offline token** and use it to generate an **access token**. These tokens allow authentication with the Red Hat API and enable downloading the AAP installation package.

#### **Steps to Obtain the Tokens:**

1. **Generate an Offline Token:**
   - Go to the [API Tokens](https://access.redhat.com/articles/3626371) page on the Red Hat portal.
   - Click the **"Generate Token"** button to create a new offline token.

   **Note:** The offline token never expires as long as it is used at least once every 30 days.

2. **Generate an Access Token:**
   - Use the offline token to obtain an access token, which is valid for 15 minutes.
   - The access token is required to authenticate with the API and download AAP.

For more details on using Red Hat APIs and obtaining tokens, refer to the official article:  
[Getting started with Red Hat APIs](https://access.redhat.com/articles/3626371).

### Obtaining the SHA-256 Checksum for AAP

To ensure the integrity of the downloaded file, you can obtain the **SHA-256 Checksum** for the AAP version you want to download at the following site:

[Red Hat AAP Downloads](https://access.redhat.com/downloads/content/480/ver=2.5/rhel---9/2.5/x86_64/product-software)

## 3. Inventory Requirements

For the playbook to function correctly, the Ansible inventory must be configured to include a group called **`automationcontroller`**, containing at least one host where AAP will be installed.

### Example Inventory Configuration:

```ini
[automationcontroller]
controller.localnet.local
```

This configuration ensures that the playbook is executed on the hosts in the `automationcontroller` group, guaranteeing the correct setup and installation of AAP.

## Example Playbook

```yaml
---
- name: Download and Configure Ansible User
  hosts: automationcontroller
  gather_facts: true
  vars:
    user_info:
      - name: ansible-user
        password: '<your openssl hash>'
        key: 'ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICGzwrDAfrzylGJsj9i9bcNnduC34FaLLJZd3uzMk/fV user@hulk'
        home: /opt/aap

  tasks:
    - name: Import role
      ansible.builtin.import_role:
        name: laurobmb.ansible_aap_download.user

    - name: Import role
      ansible.builtin.import_role:
        name: laurobmb.ansible_aap_download.download
      vars:
        aap_bundle_sha_value: 88e40728986ecaa04cf3c2820ed0f4e5c85ae88c52353cc695dd5be9deab9725
        offline_token: 88e40728986ecaa04....
```

