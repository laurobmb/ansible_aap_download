# ansible_aap_download
=========

Este role realiza o download e extração do bundle do **Ansible Automation Platform (AAP)**, garantindo a integridade do arquivo e autenticando no portal da Red Hat.

## Requirements
------------

- O sistema deve ter acesso à internet para baixar o AAP bundle.
- O usuário que executa o playbook deve ter permissões adequadas para criar diretórios e manipular arquivos.
- É necessário um **offline_token** válido do portal da Red Hat.
- O **SHA256** do bundle (`aap_bundle_sha_value`) deve ser fornecido corretamente.

## Role Variables
--------------

As principais variáveis utilizadas pelo role são:

- **`offline_token`**: Token de autenticação offline para acessar o portal da Red Hat.
- **`aap_bundle_sha_value`**: Hash SHA256 do bundle do AAP para validação do download.
- **`user_info`**: Lista contendo informações dos usuários responsáveis pelo armazenamento do bundle.
- **`bundle_file`**: Armazena os resultados da verificação do bundle no sistema.

## Dependencies
------------

Nenhuma dependência externa de outras roles no Ansible Galaxy.

## Example Playbook
----------------

Exemplo de uso do role:

```yaml
- hosts: servers
  roles:
    - { role: ansible_aap_download, offline_token: "seu_token", aap_bundle_sha_value: "seu_sha256" }
```

## License
-------

BSD

## Author Information
------------------

Criado por Lauro Gomes, responsável pela configuração de usuários no ambiente AAP.
