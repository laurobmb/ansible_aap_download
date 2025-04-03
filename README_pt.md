# Ansible Collection - laurobmb.ansible_aap_download

Os playbooks são projetados para **criar e configurar um usuário para a instalação do Ansible Automation Platform (AAP)** e **baixar e extrair o pacote de instalação do AAP**, garantindo que o ambiente esteja corretamente preparado para a implantação.

## 1. Criando e Configurando o Usuário Ansible

O primeiro conjunto de tarefas, importado via a role `laurobmb.ansible_aap_download.user`, cria um usuário de sistema específico (`ansible-user`), define sua senha e configura o acesso SSH. Além disso, ajusta permissões de sudo e gera uma chave SSH para autenticação segura.

Para definir a senha do `ansible-user`, utilize o seguinte comando para gerar uma senha criptografada:

```bash
openssl passwd -1 <sua senha>
```

O valor gerado deve ser utilizado na variável `user_info.password` dentro do playbook.

## 2. Baixando e Extraindo o Pacote do AAP

O segundo conjunto de tarefas, importado via a role `laurobmb.ansible_aap_download.download`, autentica no portal da Red Hat usando um **token offline** e verifica se o arquivo do pacote do AAP já existe no servidor. Caso contrário, o playbook faz o download diretamente do portal da Red Hat, valida seu checksum para garantir integridade e, por fim, extrai o conteúdo no diretório apropriado.

### Como Obter os Tokens de Autenticação para a API da Red Hat

Para automatizar o download do Ansible Automation Platform (AAP) usando a API da Red Hat, você precisa obter um **token offline** e utilizá-lo para gerar um **token de acesso**. Esses tokens permitem a autenticação na API da Red Hat e possibilitam o download do pacote de instalação do AAP.

#### **Passos para Obter os Tokens:**

1. **Gerar um Token Offline:**
   - Acesse a página de [API Tokens](https://access.redhat.com/articles/3626371) no portal da Red Hat.
   - Clique no botão **"Generate Token"** para criar um novo token offline.

   **Nota:** O token offline nunca expira, desde que seja utilizado pelo menos uma vez a cada 30 dias.

2. **Gerar um Token de Acesso:**
   - Utilize o token offline para obter um token de acesso, válido por 15 minutos.
   - O token de acesso é necessário para autenticar na API e fazer o download do AAP.

Para mais detalhes sobre o uso das APIs da Red Hat e obtenção de tokens, consulte o artigo oficial:  
[Getting started with Red Hat APIs](https://access.redhat.com/articles/3626371).

### Obtendo o SHA-256 Checksum do AAP

Para garantir a integridade do arquivo baixado, você pode obter o **SHA-256 Checksum** da versão do AAP que deseja fazer download no seguinte site:

[Red Hat AAP Downloads](https://access.redhat.com/downloads/content/480/ver=2.5/rhel---9/2.5/x86_64/product-software)

## 3. Requisitos do Inventário

Para que o playbook funcione corretamente, o inventário do Ansible deve estar configurado e incluir um grupo chamado **`automationcontroller`**, contendo pelo menos um host onde o AAP será instalado.

### Exemplo de Configuração do Inventário:

```ini
[automationcontroller]
controller.localnet.local
```

Essa configuração garante que o playbook seja executado nos hosts do grupo `automationcontroller`, assegurando a correta configuração e instalação do AAP.

## Exemplo de Playbook

```yaml
---
- name: Baixar e Configurar Usuário Ansible
  hosts: automationcontroller
  gather_facts: true
  vars:
    user_info:
      - name: ansible-user
        password: '<seu hash do openssl>'
        key: 'ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICGzwrDAfrzylGJsj9i9bcNnduC34FaLLJZd3uzMk/fV user@hulk'
        home: /opt/aap

  tasks:
    - name: Importar role
      ansible.builtin.import_role:
        name: laurobmb.ansible_aap_download.user

    - name: Importar role
      ansible.builtin.import_role:
        name: laurobmb.ansible_aap_download.download
      vars:
        aap_bundle_sha_value: 88e40728986ecaa04cf3c2820ed0f4e5c85ae88c52353cc695dd5be9deab9725
        offline_token: 88e40728986ecaa04....
```

