# Ansible
- Ferramenta de automatização de configurações e tarefas
- Garante idempotência
- Acesso via SSH para executar as tarefas
- Coleção de plugins para facilitar a execução de qualquer tipo de tarefa.
- É muito comum a realização do provisionamento de **infra com Terraform** e **configuração das maquinas com Ansible**

### Install
[Installing and upgrading Ansible
](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-and-upgrading-ansible)

```bash
python3 -m pip install --user ansible
```

### Arquivo de inventario
Lista (Inventario) de IPs de todas as maquinas

[./hosts](./hosts)
```bash
ansible -i hosts all -m ping

-------

localhost | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

Criando containers Ubuntu
```bash
> docker compose up -d
```

Configurando ansible no container **control**
```bash
> docker exec -it control bash
```

```bash
> cd ~/ansible
> ansible -i hosts all -m ping
localhost | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
```

```bash
> docker exec -it node1 bash
> service ssh start
```

```bash
> docker exec -it node2 bash
> service ssh start
```

```bash
> docker exec -it control bash
> ssh-keygen
> ssh-copy-id root@node1
```

```bash
> docker exec -it control bash
> ssh-copy-id root@node2
```

Testando ansible no control
```bash
> docker exec -it control bash
> cd ~/ansible
> ansible -i hosts all -m ping
> ansible -i hosts all -m shell -a 'uptime'
node1 | SUCCESS | rc=0 >>
 23:32:26 up 1 day, 11:28,  2 users,  load average: 2.35, 1.82, 1.59
```

### Instalar Git nas maquinas
```bash
> ansible -i hosts all -m apt -a "update_cache=yes name=git state=present"
node1 | SUCCESS => {
    "cache_update_time": 1688167834, 
    "cache_updated": true, 
    "changed": true, 
    "stderr": "debconf: delaying package configuration, since apt-utils is not installed\n", 
    "stderr_lines": [
        "debconf: delaying package configuration, since apt-utils is not installed"
    ], 
    "stdout": "Reading package lists...", 
    "stdout_lines": [
        "Reading package lists...", 
        ...
    ]
}
node2 | SUCCESS => {
    "cache_update_time": 1688167839, 
    "cache_updated": true, 
    "changed": true, 
    "stderr": "debconf: delaying package configuration, since apt-utils is not installed\n", 
    "stderr_lines": [
        "debconf: delaying package configuration, since apt-utils is not installed"
    ], 
    "stdout": "Reading package lists...", 
    "stdout_lines": [
        "Reading package lists...", 
        ...
    ]
}
```

### Checkout em um repositório
```bash
> ansible -i hosts all -m git -a "repo=https://github.com/robsantossilva/fullcycle-terraform dest=/root/terraform-repo"
node1 | SUCCESS => {
    "after": "6d585e1712f824e2ba1fce29430efe532b714a22", 
    "before": null, 
    "changed": true
}
node2 | SUCCESS => {
    "after": "6d585e1712f824e2ba1fce29430efe532b714a22", 
    "before": null, 
    "changed": true
}
```