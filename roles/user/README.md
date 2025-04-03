# ansible_aap_user_setup
=========

Este role cria e configura usuários no sistema para a instalação e operação do **Ansible Automation Platform (AAP)**, incluindo a criação de chaves SSH e permissões de sudo.

## Requirements
------------

- O Ansible deve ter permissões para criar usuários e modificar arquivos de configuração do sistema.
- O módulo `community.crypto` deve estar disponível para a geração de chaves OpenSSH.
- O grupo `automationcontroller` deve estar definido no inventário para ativar a geração e distribuição das chaves SSH.

## Role Variables
--------------

As principais variáveis utilizadas pelo role são:

- **`user_info`**: Lista de dicionários contendo:
  - `name`: Nome do usuário a ser criado.
  - `password`: password do usuário.
  - `home`: Diretório home do usuário.
  - `key`: Chave SSH pública para autorização.

- **`sshkey.results`**: Armazena as informações da chave SSH gerada.

## Dependencies
------------

Nenhuma dependência externa de outras roles no Ansible Galaxy.

## Example Playbook
----------------

Exemplo de uso do role:

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

Criado por Lauro Gomes, responsável pela configuração de usuários no ambiente AAP.
