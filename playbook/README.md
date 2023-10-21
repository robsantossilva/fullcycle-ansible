# Iniciando com Playbook

```bash
ansible-playbook -i hosts playbook.yaml
```

## Ansible Galaxy

### Roles

```bash
> cd ~/roles
> ansible-galaxy init install_nginx
#- Role install_nginx was created successfully
```

./roles/install_nginx/tasks/main.yml
```yaml
- name: Install nginx
  apt:
    name: nginx
    state: present
    update_cache: yes

- name: Init nginx
  service:
    name: nginx
    state: started
```

**Criando arquivo entrypoint**
./roles/main.yaml
```yaml
---
- hosts: all
  remote_user: ubuntu
  become: true
  roles:
    - install_nginx
```

```bash
> cd ~/roles
> ansible-playbook -i ../hosts main.yaml
```

## Instalando Docker
```bash
> cd ~/roles
> ansible-galaxy init install_docker
#- Role install_docker was created successfully
```

./roles/install_docker/tasks/main.yml

## Install docker swarm manager
```bash
> cd ~/roles
> ansible-galaxy init docker_swarm_manager
```

### Run playbook
```bash
> cd ~/roles
> ansible-playbook -i ../hosts main.yaml
```

## Install docker swarm worker
```bash
> cd ~/roles
> ansible-galaxy init docker_swarm_worker
```

## Validar workers
- Entrar na maquina manager e rodar o comando:
```bash
docker node ls
```

## Install deploy_stack
```bash
> cd ~/roles
> ansible-galaxy init deploy_stack
```

## Validar stack
- Entrar na maquina manager e rodar o comando:
```bash
> docker service ls
```

Ver replicas
```bash
> docker service ps app_app
```

Escalar containers
```bash
> docker service scale app_app=6
```

