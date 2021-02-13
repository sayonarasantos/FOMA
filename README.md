# tcc-automation

- Criar usuário com permissão de sudo com a mesma senha no master e no node:
```
adduser cdmin
usermod -aG sudo
```

- Alterar os hostnames ou ips em inventory/cluster1/hosts.ini

- Alterar o hostname ou ip do ansible e o usuário em inventory/cluster1/group_vars/all.yml

- Na maquina do ansible, executar o seguinte comando e digitar a senha do usuário nas máquinas:
```
ansible-playbook build_k3s.yml -i inventory/cluster1/hosts.ini
ansible-playbook build_management.yml -i inventory/cluster1/hosts.ini
```

---
- Comandos extras
```
ansible-playbook create_user.yml --user sayonara --ask-become-pass
ansible 192.168.15.4 -m setup -i inventory/cluster1/hosts.ini > debug.txt
ansible k3s_node -m setup -i inventory/cluster1/hosts.ini | grep buster
```
