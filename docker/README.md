# Установка Docker и Docker compose v2
## Поддерживаемые платформы
- Ubuntu 20.04
- Ubuntu 22.04
- Debian 10
- Debian 11
 ---
 ## Запуск
 ```sh
 ansible-playbook docker.yml -i <hostname>, -u <username> -kK
 ```
 ### Установка Docker
 ### Выпуск докера

 "ce" (community edition) или "ee" (enterprise edition)
 ```yml
 docker__edition: "ce"
 ```
 ### Выбор сборки
 В поле можно подставить разные варианты "stable", "edge", "testing" or "nightly"
 ```yml
 docker__channel: ["stable"]
 ```
 Тут указываем необходимую версию, если указано пустое значение, то будет установлена последняя версия в репо.
 ```yml
 docker__version: ""

# Для примера установим версию 20.10
docker__version: "20.10"

# Или более точное указание релиза
docker__version: "20.10.17"
 ```

### Обновление Docker

- Если ставим значение `"present"`, то повторный запуск роли не будет ставить новую версию.

- Если ставим значение `"latest"`, то запуск роли вызовет установку более новых версий

 ```yml
 docker__state: "present"
 ```

### Откат к старым версиям

Самый лучший способ откатиться - это удалить пакеты Docker вручную, а затем запустить эту таску с нужной версией.

```sh
# Ad-hoc команда Ansible для остановки и удаления пакетов Docker

ansible all -i '<hostname>,' -u <username> -m systemd -a "name=docker-ce state=stopped" -m apt -a "name=docker-ce autoremove=true purge=true statte=absent" -b -kK
```

### Установка Docker compose v2

Docker compose v2 устанавливается с помощью apt из официального репозитория `docker-compose-plugin`

#### Выбор версии
 Тут указываем необходимую версию, если указано пустое значение, то будет установлена последняя версия в репо
```yml
docker__compose_v2_version: ""

docker__compose_v2_version: "2.6"

docker__compose_v2_version: "2.6.0"
```

#### Обновление Docker compose v2

Для обновления используется перменная `docker__state`, которая описана выше в разделе с Docker

#### Откат к старой версии

Как и сам Docker лучше удалять compose вручную.
```sh
ansible all -i '<hostname>,' -u <username> -m apt -a "name=docker-compose-plugin autoremove=true purge=true state=absent" -b -kK
```
### Установка Docker compose v1

Разница версий не только в изменениях, но и в вызове командны v2 вызывается docker compose (без девиса), а v1 - docker-compose.

#### Указание версии
Принцип такой же как и с установкой Docker и Docker-compose v2
```yml
docker__compose_version: ""

docker__compose_version: "1.29"

docker__compose_version: "1.29.2"
```
#### Как и где указать

### Установка пакетов Python и PIP
#### Настройка VirtualEnv
Чтобы не ставиться рядом с основной версией Python на ВМ, пакеты PIP устанавливаются в отдельную виртуальную среду
```yml
docker__pip_virtualenv: "/usr/local/lib/docker/virtualenv"
```
#### Значение для пакетов PIP

`"present"` - пакет будет установлен, но не будет обновляться

`"forceinstall"` - пакет будет переустанавливаться при каждом запуске

`"absent"` - пакет будет удален

```yml
docker__pip_docker_state: "present"
docker__pip_docker_compose_state: "present"
```
### Пропустить установку Docker compose v1

Выставляем `docker__pip_docker_compose_state: "absent"`, сейчас это значение установлено по умолчанию.





