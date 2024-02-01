# Практическое задание 9. Многоэтапные сборки. Хостинг сайта Wordpress.

## Задача: развернуть сайт Wordpress

Развернуть систему `CMS` под названием `Wordpress`.

> WordPress — это бесплатный инструмент для ведения блогов с открытым исходным кодом и система управления контентом (CMS) на основе PHP и MySQL, работающая на веб-хостинге.

Требуется два контейнера:

- Один контейнер, который обслуживает PHP-файлы Wordpress.
— Один контейнер, который служит базой данных MySQL для Wordpress.

Оба контейнера  в DockerHub: [Wordpress](https://hub.docker.com/_/wordpress/) и [Mysql](https://hub.docker.com/_/mysql/).


## Ход работы

Чтобы запустить контейнер MySQL, введите следующую команду

```bash
docker run --name mysql-container --rm -p 3306:3306 -e MYSQL_ROOT_PASSWORD=wordpress -e MYSQL_DATABASE=wordpressdb -d mysql:5.7.36
```

- `docker` запускает механизм `docker engine`.
- `run` сообщает докеру запустить новый контейнер из образа.
- `--name mysql-container` дает новому контейнеру имя для более удобного обращения.
- `--rm` сообщает докеру удалить контейнер после его остановки.
- `-p 3306:3306` подключает порт хоста 3306 к порту контейнера 3306.
- `-e MYSQL_ROOT_PASSWORD=wordpress` Опция `-e` используется для определенияпеременных среды в контейнере.
- `-e MYSQL_DATABASE=wordpressdb` обозначает имя базы данных, созданной при запуске MySQL.
- `-d` запускает контейнер в фоновом режиме.
- `mysql` сообщает, какой контейнер на самом деле запускать, здесь mysql:latest (:latest — значение по умолчанию, если ничего не указано).

MySQL теперь открывает свой порт 3306 на хосте, и каждый может подключиться к нему **поэтому не делайте этого в рабочей среде без соответствующих настроек безопасности**.

Подключить WordPress-контейнер к IP-адресу хоста.

После того, как определен IP-адрес, развернуть контейнер WordPress с IP-адресом хоста в качестве переменной:

```bash
docker run --name wordpress-container --rm -e WORDPRESS_DB_HOST=172.17.0.1 -e WORDPRESS_DB_PASSWORD=wordpress -e WORDPRESS_DB_USER=root -e WORDPRESS_DB_NAME=wordpressdb -p 8080:80 -d wordpress:5.7.2-apache
```
Перейти по IP: 8080 и запустить сервер WordPress. Поскольку порт 3306 является портом MySQL по умолчанию, WordPress попытается подключиться к этому порту самостоятельно.

- Остановить два контейнера `docker stop wordpress-container mysql-container`

## Создание контейнерной сети

Несмотря на то, что с помощью двух команд выполнили настройку по приведенному выше сценарию, здесь есть некоторые проблемы, которые **НЕОБХОДИМО** исправить:

- требуется знать IP-адрес хоста, чтобы они могли общаться друг с другом.
- открыть базу данных для внешних подключений.

Чтобы подключить несколько докер-контейнеров без привязки их к сетевому интерфейсу хостов, необходимо создать `docker network`.

Команда `docker network` обеспечивает безопасное подключение и предоставляет канал для передачи информации из одного контейнера в другой.

Прежде всего создайте новую сеть для связи контейнеров:

```bash
`docker network create if_wordpress`
```

Docker вернет `networkID` для вновь созданной сети. Теперь можно ссылаться на него как по имени, так и по идентификатору.

Подключить два контейнера к сети, добавив опцию `--network`:

```bash
docker run --name mysql-container --rm --network if_wordpress -e MYSQL_ROOT_PASSWORD=wordpress -e MYSQL_DATABASE=wordpressdb -d mysql:5.7.36

docker run --name wordpress-container --rm --network if_wordpress -e WORDPRESS_DB_HOST=mysql-container -e WORDPRESS_DB_PASSWORD=wordpress -e WORDPRESS_DB_USER=root -e WORDPRESS_DB_NAME=wordpressdb -p 8080:80 -d wordpress:5.7.2-apache
```

Обратить внимание на переменную env `WORDPRESS_DB_HOST`. Когда  подключаете контейнер к сети, он автоматически получает имя контейнера в качестве DNS-имени, что упрощает обнаружение контейнерами друг друга. Имя DNS видно только внутри сети Docker, что также справедливо для назначенного им IP-адреса (обычно адреса, начинающегося с «172»).

Изучить контейнерную сеть, выполнив команду `docker network Inspect if_wordpress`.


```bash
docker network inspect if_wordpress
```

Expected output:

```json
[
  {
    "Name": "if_wordpress",
    "Id": "04e073137ff8c71b9a040ba452c12517ebe5a520960a78bccb7b242b723b5a21",
    "Created": "2017-11-28T17:20:37.83042658+01:00",
    "Scope": "local",
    "Driver": "bridge",
    "EnableIPv6": false,
    "IPAM": {
      "Driver": "default",
      "Options": {},
      "Config": [
        {
          "Subnet": "172.18.0.0/16",
          "Gateway": "172.18.0.1"
        }
      ]
    },
    "Internal": false,
    "Attachable": false,
    "Ingress": false,
    "Containers": {
      "af38acac52301a7c9689d708e6c3255704cdffb1972bcc245d67b02840983a50": {
        "Name": "mysql-container",
        "EndpointID": "96b4befec46c788d1722d61664e68bfcbd29b5d484f1f004349163249d28a03d",
        "MacAddress": "02:42:ac:12:00:02",
        "IPv4Address": "172.18.0.2/16",
        "IPv6Address": ""
      },
      "fb4dad5cd82b5b40ee4f7f5f0249ff4b7b4116654bab760719261574b2478b52": {
        "Name": "wordpress-container",
        "EndpointID": "2389930f52893e03a15fdc28ce59316619cb061e716309aa11a2716ef09cde17",
        "MacAddress": "02:42:ac:12:00:03",
        "IPv4Address": "172.18.0.3/16",
        "IPv6Address": ""
      }
    },
    "Options": {},
    "Labels": {}
  }
]
```
Поскольку  оба контейнера связаны в одну сеть, теперь к контейнеру WordPress можно получить доступ из браузера по адресу [http://localhost:8080](http://localhost:8080), и настройку WordPress можно легко выполнить. MySQL недоступен извне, поэтому безопасность стала намного лучше, чем раньше.

### Очистка

Закройте оба контейнера, выполнив следующую команду:

```bash
docker stop wordpress-container mysql-container
```

## Использование Docker Compose

Если создавать образы контейнеров для своих служб приложений,  через некоторое время вможет понадобиться писать длинные команды запуска контейнера docker.
Эти команды, хотя и очень интуитивно понятны, могут оказаться громоздкими для написания, особенно если разрабатывается многоконтейнерные приложения и требуется быстрое развертывание контейнеров.

[Docker Compose](https://docs.docker.com/compose/install/) — это «_инструмент для определения и запуска ваших многоконтейнерных приложений Docker_».

Ваши приложения могут быть определены в файле YAML, в котором определены все параметры, которые вы использовали в `docker run`.

Compose также позволяет вам управлять вашим приложением как единым объектом, а не работать с отдельными контейнерами.

Этот файл определяет все контейнеры и настройки, необходимые для запуска набора кластеров. Однако свойства соответствуют тому, как вы используете команды запуска Docker, теперь хранятся в системе контроля версий и доступны вместе с вашим кодом.

If you have started working with Docker and are building container images for your application services, you most likely have noticed that after a while you may end up writing long `docker container run` commands.
These commands, while very intuitive, can become cumbersome to write, especially if you are developing a multi-container applications and spinning up containers quickly.

[Docker Compose](https://docs.docker.com/compose/install/) is a “_tool for defining and running your multi-container Docker applications_”.

Your applications can be defined in a YAML file where all the options that you used in `docker run` are defined.

Compose also allows you to manage your application as a single entity rather than dealing with individual containers.

This file defines all of the containers and settings you need to launch your set of clusters. The properties map onto how you use the docker run commands, however, are now stored in source control and shared along with your code.

## Terminology

- `docker-compose.yml` The YAML file where all your configuration of your docker containers go.
- `docker-compose` The cli tool that enables you to define and run multi-container applications with Docker

  - `up` : creates and starts the services stated in the compose file
  - `down` : stops and removes containers, networks, images, and volumes
  - `restart` :
  - `logs` : streams the acummulated logs from all the containers in the compose file
  - `ps` : same as `docker ps`; shows you all containers that are currently running.
  - `rm` : removes all the containers from the given compose file.
  - `start` : starts the services
  - `stop` : stops the services

The docker cli is used when managing individual containers on a docker engine.
It is the client command line to access the docker daemon api.

The docker-compose cli together with the yaml files can be used to manage a multi-container application.

## Compose-erizing your wordpress

So we want to take advantage of docker-compose to run our wordpress site.

In order to to this we need to:

1. Transform our setup into a docker-compose.yaml file
1. Invoke docker-compose and watch the magic happen!

Head over to this labs folder:

`cd labs/multi-container`

Open the file `docker.compose.yaml` with a text editor:

`nano docker-compose.yaml`

You should see something like this:

```yaml
version: "3.1"

services:
  #  wordpress_container:

  mysql_container:
    image: mysql:5.7
    ports:
      - 3306:3306
    environment:
      MYSQL_ROOT_PASSWORD: wordpress
```

This is the template we are building our compose file upon so let's drill this one down:

- `version` indicate what version of the compose syntax we are using
- `services` is the section where we put our containers
  - `wordpress_container` is the section where we define our wordpress container
  - `mysql_container` is the ditto of MySQL.

> For more information on docker-compose yaml files, head over to the [documentation](https://docs.docker.com/compose/overview/).

The `services` part is equivalent to our `docker container run` command. Likewise there is a `network` and `volumes` section for those as well corresponding to `docker network create` and `docker volume create`.

Let's look the mysql_container part together, making you able to create the other container yourself. Look at the original command we made to spin up the container:

`docker container run --name mysql-container --rm -p 3306:3306 -e MYSQL_ROOT_PASSWORD=wordpress -e MYSQL_DATABASE=wordpressdb -d mysql:5.7.36`

The command gives out following information: a `name`, a `port` mapping, two `environment` variables and the `image` we want to run.

Now look at the docker-compose example again:

- `mysql_container` defines the name of the container
- `image:wordpress` describes what image the container spins up from.
- `ports` defines a list of port mappings from host to container
- `environment` describes the `-e` variable made before in a yaml list

Instead of keeping sensitive information in the `docker-compose.yml` file, you can also use an [`.env`](https://docs.docker.com/compose/env-file/) file to keep all the environment variables. That way, it's easier to make a development environment and a production environment with the same `docker-compose.yml`.

```conf
MYSQL_ROOT_PASSWORD=wordpress
```

Try to spin up the container in detached mode:

```bash
docker-compose up -d
Creating network "multicontainer_default" with the default driver
Creating multicontainer_mysql_container_1 ...
Creating multicontainer_mysql_container_1 ... done
```

Looking at the output you can see that it made a `docker network` named `multicontainer_default` as well as the MySQL container named `multicontainer_mysql_container_1`.

Issue a `docker container ls` as well as `docker network ls` to see that both the container and network are listed.

To shut down the container and network, issue a `docker-compose down`

> **note**: The command docker-compose down removes the containers and default network.

### Creating the wordpress container

You now have all the pieces of information to make the Wordpress container. We've copied the run command from before if you can't remember it by heart:

```bash
docker run --name wordpress-container --rm --network if_wordpress -e WORDPRESS_DB_HOST=mysql-container -e WORDPRESS_DB_PASSWORD=wordpress -e WORDPRESS_DB_USER=root -e WORDPRESS_DB_NAME=wordpressdb -p 8080:80 -d wordpress:5.7.2-apache
```

You must

- uncomment the `wordpress_container` part of the services section
- map the pieces of information from the docker container run command to the yaml format.
- remove MySQL port mapping to close that from outside reach.

When you made that, run `docker-compose up -d` and access your wordpress site from [http://IP:8080](http://IP:8080)

> **Hint**: If you are stuck, look at the file docker-compose_final.yaml in the same folder.
