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
ansible-playbook site.yml -i inventory/cluster1/hosts.ini --ask-become-pass
```