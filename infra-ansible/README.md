# Ansible Infrastructure 

## Назначение

Данный проект предназначен для:

- Подготовки серверов Ubuntu
- Настройки безопасного SSH
- Создания административных и технических пользователей
- Установки Docker фиксированной версии из стандартного репозитория Ubuntu
- Установки необходимого ПО
- Ввода серверов в кластер Docker Swarm
- Назначения worker-нодам label `role=worker`

---
## Структура
infra-ansible
├── ansible.cfg
├── inventories
│   └── prod
│       ├── group_vars
│       │   └── all.yml
│       └── hosts.yml
├── playbooks
│   ├── prepare_servers.yml
│   └── swarm.yml
├── README.md
└── roles
    ├── base
    │   ├── handlers
    │   │   └── main.yml
    │   ├── tasks
    │   │   └── main.yml
    │   └── templates
    │       └── sshd_config.j2
    └── docker
        ├── handlers
        │   └── main.yml
        └── tasks
            └── main.yml

## Требования

### Управляющая машина

- Ansible >= 2.14
- Python >= 3.10
- SSH-доступ по ключу
- Пользователь подключения — `ansible`
- Sudo без запроса пароля

### Установка коллекции Docker

```bash
ansible-galaxy collection install community.docker

```
## Запуск
Подготовка серверов в установке докера
```
ansible-playbook playbooks/prepare_servers.yml
```
Установка докера
```
ansible-playbook playbooks/swarm.yml
```
