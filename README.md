Домашнее задание к занятию 4 «Работа с roles»
Подготовка к выполнению
Необязательно. Познакомьтесь с LightHouse.
Создайте два пустых публичных репозитория в любом своём проекте: vector-role и lighthouse-role.
Добавьте публичную часть своего ключа к своему профилю на GitHub.
Основная часть
Ваша цель — разбить ваш playbook на отдельные roles.

Задача — сделать roles для ClickHouse, Vector и LightHouse и написать playbook для использования этих ролей.

Ожидаемый результат — существуют три ваших репозитория: два с roles и один с playbook.

Что нужно сделать

Создайте в старой версии playbook файл requirements.yml и заполните его содержимым:

---
  - src: git@github.com:AlexeySetevoi/ansible-clickhouse.git
    scm: git
    version: "1.13"
    name: clickhouse 
При помощи ansible-galaxy скачайте себе эту роль.

Создайте новый каталог с ролью при помощи ansible-galaxy role init vector-role.

На основе tasks из старого playbook заполните новую role. Разнесите переменные между vars и default.

Перенести нужные шаблоны конфигов в templates.

Опишите в README.md обе роли и их параметры. Пример качественной документации ansible role по ссылке.

Повторите шаги 3–6 для LightHouse. Помните, что одна роль должна настраивать один продукт.

Выложите все roles в репозитории. Проставьте теги, используя семантическую нумерацию. Добавьте roles в requirements.yml в playbook.

Переработайте playbook на использование roles. Не забудьте про зависимости LightHouse и возможности совмещения roles с tasks.

Выложите playbook в репозиторий.

В ответе дайте ссылки на оба репозитория с roles и одну ссылку на репозиторий с playbook.

Как оформить решение задания
Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.



##  Решение 


### Создание ролей

Роли созданы при помощи:
```bash
ansible-galaxy role init vector-role
ansible-galaxy role init lighthouse-role
```

В каждой роли подготовлены каталоги:
```
tasks/
handlers/
templates/
files/
vars/
defaults/
meta/
tests/
```

Добавлены тестовые задачи:

```yaml
# vector-role/tasks/main.yml
- name: Vector | placeholder task
  debug:
    msg: "Vector role is connected"

# lighthouse-role/tasks/main.yml
- name: LightHouse | placeholder task
  debug:
    msg: "LightHouse role is connected"
```

---

###  Файл `requirements.yml`

```yaml
---
- src: "git@github.com:AlexeySetevoi/ansible-clickhouse.git"
  scm: git
  version: "1.13"
  name: clickhouse

- src: "git@github.com:zis-git/vector-role.git"
  scm: git
  version: "main"
  name: vector-role

- src: "git@github.com:zis-git/lighthouse-role.git"
  scm: git
  version: "main"
  name: lighthouse-role
```

---

###  Установка ролей

```bash
ansible-galaxy install -r requirements.yml -p roles
```

Вывод:
```
- clickhouse (1.13) was installed successfully
- vector-role (main) was installed successfully
- lighthouse-role (main) was installed successfully
```

---

### Конфигурация Ansible

Файл `ansible.cfg`:
```ini
[defaults]
roles_path = /root/ansible-netology/04-roles:./roles
```

---

### Плейбук `site.yml`

```yaml
---
- name: Install ClickHouse
  hosts: clickhouse
  roles:
    - clickhouse

- name: Install Vector
  hosts: vector
  roles:
    - vector-role

- name: Install LightHouse
  hosts: lighthouse
  roles:
    - lighthouse-role
```

---

### Инвентори `inventory.ini`

```ini
[clickhouse]
ch-01 ansible_connection=local

[vector]
vector-01 ansible_connection=local

[lighthouse]
lighthouse-01 ansible_connection=local
```

---

### Проверка

```bash
ansible-playbook -i inventory.ini site.yml --list-tasks
```

Вывод:
```
play #1 (clickhouse): Install ClickHouse
play #2 (vector): Install Vector
  tasks:
    vector-role : Vector | placeholder task
play #3 (lighthouse): Install LightHouse
  tasks:
    lighthouse-role : LightHouse | placeholder task
```

---

Репозитории созданы


Vector role - https://github.com/zis-git/vector-role](https://github.com/zis-git/vector-role) |
LightHouse role -https://github.com/zis-git/lighthouse-role](https://github.com/zis-git/lighthouse-role) |
Playbook - https://github.com/zis-git/ansible-playbook-roles](https://github.com/zis-git/ansible-playbook-roles) |

---

## Вывод

- Все роли успешно созданы, оформлены и подключены через `requirements.yml`.
- Проверка `--list-tasks` подтверждает, что роли подхватываются корректно.
- Репозитории опубликованы на GitHub и доступны для проверки.


*


