# Практическое задание 9. Многоэтапные сборки. Хостинг сайта Wordpress.

## Задача: развернуть сайт Wordpress

Развернуть систему `CMS`  Wordpress.

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

[Docker Compose](https://docs.docker.com/compose/install/) — это «_инструмент для определения и запуска многоконтейнерных приложений Docker_».

Приложения могут быть определены в файле `YAML`, в котором определены все параметры, которые используются в `docker run`.

Compose также позволяет управлять приложением как единым объектом, а не работать с отдельными контейнерами.

Этот файл определяет все контейнеры и настройки, необходимые для запуска набора кластеров. Однако свойства теперь хранятся в системе контроля версий и доступны вместе с  кодом.


## Терминология

- `docker-compose.yml` Файл YAML, в котором хранятся все настройки ваших Docker-контейнеров.
- `docker-compose` Инструмент командной строки, который позволяет определять и запускать многоконтейнерные приложения с помощью Docker.

   - `up`: создает и запускает службы, указанные в файле компоновки.
   - `down`: останавливает и удаляет контейнеры, сети, образы и тома.
   - `перезапуск`:
   - `logs`: передает журналы из всех контейнеров в файле компоновки.
   - `ps`: то же, что и `docker ps`; показывает все контейнеры, которые в данный момент работают.
   - `rm`: удаляет все контейнеры из данного файла компоновки.
   - `start`: запускает службы
   - `stop`: останавливает службы.

`Docker Cli` используется при управлении отдельными контейнерами в механизме Docker.
Это командная строка клиента для доступа к API-интерфейсу демона Docker.

`Docker-compose cli` вместе с файлами `yaml` можно использовать для управления многоконтейнерным приложением.

## Сборка сервиса WordPress

Использовать `docker-compose` для запуска  сайта на WordPress.

Для этого необходимо:

1. Перенести автонастройку в файл `docker-compose.yaml`.
1. Вызвать `docker-compose`.

Перейдите в директорию:

```bash
cd labs/multi-container
```

Открть файл `docker.compose.yaml` в текстовом редакторе:

```bash
nano docker-compose.yaml
```
Ожидаемый результат:

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
Это шаблон, на основе которого создается файл компоновки:

- `version` указывает, какую версию синтаксиса сборки используем.
— `services` — это раздел, куда помещаются контейнеры.
   — «wordpress_container» — это раздел, в котором определяется контейнер WordPress.
   - `mysql_container` — это раздел, в котором определяется контейнер MySQL.

> Для получения дополнительной информации о файлах yaml, создаваемых docker, перейдите к [документации](https://docs.docker.com/compose/overview/).

Часть `services` эквивалентна команде запуска docker-контейнера. Аналогично, есть разделы `network` и `volumes` для тех, которые также соответствуют `docker network create` и `docker volume create`.

Рассмотреть часть `mysql_container`, что позволит создать другой контейнер самостоятельно

`docker container run --name mysql-container --rm -p 3306:3306 -e MYSQL_ROOT_PASSWORD=wordpress -e MYSQL_DATABASE=wordpressdb -d mysql:5.7.36`

Команда выдает следующую информацию: имя, сопоставление порта, две переменные среды и образ, которые требуется запустить.

Объекты `docker-compose`:

- `mysql_container` определяет имя контейнера.
- `image:wordpress` описывает, из какого изображения запускается контейнер.
- `ports` определяет список сопоставлений портов от хоста к контейнеру.
- `environment` описывает переменную `-e`, созданную ранее в списке yaml.

Вместо хранения информации в файле `docker-compose.yml` также можно использовать файл [`.env`](https://docs.docker.com/compose/env-file/), чтобы хранить всю переменные среды. Таким образом, будет проще создать среду разработки и производственную среду с одним и тем же файлом `docker-compose.yml`.

```conf
MYSQL_ROOT_PASSWORD=wordpress
```

Развернуть контейнер:

```bash
docker-compose up -d
Creating network "multicontainer_default" with the default driver
Creating multicontainer_mysql_container_1 ...
Creating multicontainer_mysql_container_1 ... done
```
Созданы «docker network» с именем «multicontainer_default», а также контейнер MySQL с именем «multicontainer_mysql_container_1».

Выполнить команду `docker container ls`, а также `docker network ls`, чтобы увидеть, что и контейнер, и сеть указаны в списке.

Чтобы завершить работу контейнера и сети, выполните команду `docker-compose down`.

> **примечание**: команда `docker-compose down` удаляет контейнеры и сеть по умолчанию.

### Индивидуальное задание. Создание контейнера WordPress

Развернуть  и настроить систему `CMS`  Wordpress на основе файл [docker-compose_final.yaml](https://github.com/BosenkoTM/UC-WD/blob/main/docker-compose/labs/multi-container/docker-compose_final.yaml).

Выполнить команду docker-compose up -d и зайдите на свой сайт WordPress с [http://IP:8080](http://IP:8080).

