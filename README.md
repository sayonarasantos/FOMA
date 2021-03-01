# tcc-automation

## Pré-requisitos
- Versão do Ansible: 2.9
- Python instalados em todas as máquinas
- Usuário com permissão de sudo, mesma senha nos nós e acesso via chave SSH nas máquinas

## Configuração das variáveis
- Criar um arquivo inventory/cluster1/hosts.ini, copiando o arquivo inventory/cluster1/m_hosts.ini
- Alterar os ips em inventory/cluster1/hosts.ini
- Criar um arquivo inventory/cluster1/group_vars/all.yml, compiando o arquivo inventory/cluster1/group_vars/m_all.yml
- Alterar as variáveis necessárias em inventory/cluster1/group_vars/all.yml, como:
    - k3s_version: versão do K3s,
    - ansible_user: usuário ansible de acesso,
    - master_on_manager_machine: parâmetro para confirmar a configuração de acesso remoto ao cluster K3s
    - ansible_become_password: senha do usuário ansible de acesso
    - influxdb_*: configurações do banco de dados 

## Execução dos playbooks
- Na maquina do ansible, executar o seguinte comando e digitar a senha do usuário nas máquinas:
```
ansible-playbook build_k3s.yml -i inventory/cluster1/hosts.ini
ansible-playbook build_management.yml -i inventory/cluster1/hosts.ini
```




---

## Comandos extras

- Em cada máquina, criar usuário com permissão de sudo e mesma senha nos nós:
```
sudo adduser cdmin
usermod -aG sudo cdmin
```

- Verificar informações das máquinas
```
ansible 192.168.15.4 -m setup -i inventory/cluster1/hosts.ini > debug.txt
ansible k3s_node -m setup -i inventory/cluster1/hosts.ini | grep buster
```

- Criar um novo usuário
```
ansible-playbook create_user.yml --user <usuário remoto> --ask-become-pass
```