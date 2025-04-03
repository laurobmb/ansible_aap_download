# Ansible Collection - laurobmb.ansible_aap_download

Os playbooks têm como objetivo principal a **criação e configuração de um usuário para a instalação do Ansible Automation Platform (AAP)** e o **download e extração do bundle de instalação do AAP**, garantindo que o ambiente esteja preparado corretamente para a implantação.  

## 1. Criação e Configuração do Usuário Ansible  
O primeiro conjunto de tarefas, importado pelo role `laurobmb.ansible_aap_download.user`, cria um usuário específico (`ansible-user`) no sistema, define sua password e configura o acesso SSH. Além disso, ajusta permissões de sudo e gera uma chave SSH para autenticação segura.  

Para definir a password do usuário `ansible-user`, deve-se utilizar o seguinte comando para gerar a password criptografada:  

```bash
openssl passwd -1 <your password>
```

O valor gerado deve ser utilizado na variável `user_info.password` dentro do playbook.  

## 2. Download e Extração do AAP Bundle  
O segundo conjunto de tarefas, importado pelo role `laurobmb.ansible_aap_download.download`, realiza a autenticação no portal da Red Hat utilizando um **offline token** e verifica se o arquivo do bundle do AAP já existe no servidor. Caso contrário, o playbook baixa o arquivo diretamente do portal da Red Hat, valida seu checksum para garantir integridade e, por fim, extrai o conteúdo no diretório apropriado.  

### Como obter os tokens para autenticação na API da Red Hat  

Para automatizar o download do Ansible Automation Platform (AAP) utilizando a API da Red Hat, é necessário obter um **token offline** e, a partir dele, gerar um **token de acesso**. Esses tokens permitem autenticar-se na API da Red Hat e realizar o download do bundle de instalação do AAP.  

#### **Passos para obter os tokens:**  

1. **Gerar um Token Offline:**  
   - Acesse a página de [API Tokens](https://access.redhat.com/articles/3626371) da Red Hat.  
   - Clique no botão **"Generate Token"** para criar um novo token offline.  

   **Observação:** O token offline nunca expira, desde que seja utilizado pelo menos uma vez a cada 30 dias.  

2. **Gerar um Token de Acesso:**  
   - Utilize o token offline para obter um token de acesso válido por 15 minutos.  
   - O token de acesso é necessário para autenticar-se na API e realizar o download do AAP.  

Para mais detalhes sobre o uso das APIs da Red Hat e a obtenção dos tokens, consulte o artigo oficial:  
[Getting started with Red Hat APIs](https://access.redhat.com/articles/3626371).  

## 3. Requisitos de Inventário  
Para que o playbook funcione corretamente, é necessário que o inventário do Ansible esteja configurado e inclua um grupo chamado **`automationcontroller`**, contendo pelo menos um host onde o AAP será instalado.  

### Exemplo de configuração do inventário:  

```ini
[automationcontroller]
controller.localnet.local 
```

Dessa forma, o playbook pode ser executado contra os hosts do grupo `automationcontroller`, garantindo que a configuração e a instalação do AAP ocorram corretamente.

## Playbook de Exemplo

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
