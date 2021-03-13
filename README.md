# FOMA - Framework de Orquestração, Monitoramento e Automatização de ambiente de Névoa
Autora: Sayonara Santos Araújo

Descrição: O FOMA é um framework para ambientes de Névoa, que permite a orquestração de serviços, o monitoramento de recursos e a implantação automatizada das ferramentas da infraestrutura. O framework é composto pelas ferramentas:
- K3s e kubectl para a orquestração de contêiner
- TIG (Telegraf, InfluxDB e Grafana) para o monitoramento das máquinas
- Ansible para a implantação automatizada das ferramentas de orquestração e monitoramento

## Pré-requisitos
- Versão do Ansible: 2.9
- Python instalados em todas as máquinas
- Usuário com permissão de sudo, mesma senha nos nós e acesso via chave SSH nas máquinas

## Configuração das variáveis
Antes de executar os playbooks, é necessário configurar as variáveis do inventário, seguindos os passos:
- Criar um arquivo `inventory/cluster1/hosts.ini`, copiando o arquivo `inventory/cluster1/m_hosts.ini`
- Alterar os ips em `inventory/cluster1/hosts.ini`
- Criar um arquivo `inventory/cluster1/group_vars/all.yml`, compiando o arquivo `inventory/cluster1/group_vars/m_all.yml`
- Alterar as variáveis necessárias em `inventory/cluster1/group_vars/all.yml`, como:
    - ansible_user: usuário ansible de acesso,
    - ansible_become_password: senha do usuário ansible de acesso
    - ansible_ssh_pass: senha da chave ssh
    - k3s_version: versão do K3s,
    - install_kubectl: parâmetro para confirmar a configuração de acesso remoto ao cluster K3s
    - influxdb_*: configurações do banco de dados

## Execução dos playbooks
Feita a configuração das variáveis, pode-se executar os playbooks de construção do cluster K3s e implantação do TIG e do kubectl, respectivamente:
```
ansible-playbook build_k3s.yml -i inventory/cluster1/hosts.ini
ansible-playbook build_management.yml -i inventory/cluster1/hosts.ini
```

## Recursos disponibilizados pelo FOMA após a implantação
Após a implantação, você terá:
- O cluster K3s instalado e configurado em todos os nós (`k3s_node`)
- O kubectl instalado e configurado para gerenciar o cluster remotamente, a partir da máquina `manager_machine`
- O InfluxDB instalado e configurado com dois usuário (um administrador e outro para o Telegraf) na máquina `monitoring_service`
- O banco de dados do Telegraf criado no InfluxDB
- O Telegraf instalado nos nós e configurado com as informações do banco
- o Grafana instalado na máquina `monitoring_service` e disponível em http://<ip da monitoring_service>:3000

---

## Comandos extras

- Verificar informações das máquinas (exemplos)
```
ansible 192.168.15.2 -m setup -i inventory/cluster1/hosts.ini > debug.txt
ansible k3s_node -m setup -i inventory/cluster1/hosts.ini | grep buster
```
