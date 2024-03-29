# Практическое задание 3. Сборка контейнера.

Поскольку контейнеры — это всего лишь тонкий базовый слой поверх ядра хоста, новый экземпляр можно очень быстро развернуть, если поврежден старый.

Запустить  контейнер `alpine` и удалить файловую систему.

Запустите контейнер с помощью `docker run -ti alpine`, а затем провести анализ папок в корневом каталоге:

```
ls /
```

Ожидаемый результат:

```
bin    etc    lib    mnt    root   sbin   sys    usr
dev    home   media  proc   run    srv    tmp    var
```
Список текущего пользователя:
``` bash
whoami
```
Ожидаемый результат:

```
root
```

Проверить текущую дату:
``` bash
date
```

Ожидаемый результат:

```
Sat Jan 27 21:44:17 UTC 2024
```


> **Внимание!** Убедитесь, что находитесь внутри контейнера. Это можно увидеть по командной строке, отображающей `/ #` вместо `ubuntu@cloudshell:~/cloudshell_open$`

Теперь удалите двоичные файлы, из которых состоит система::

```
rm -rf /bin
```
Попробуйте изучить файловую систему, чтобы увидеть, какая часть ОС исчезла:  запустить команды `ls`, `whoami` и `date`.
Все они должны ответить, что двоичный файл не найден.

Выйти из контейнера, нажав `Ctrl+d`, создайте новый экземпляр изображения Alpine:
```
docker run -it alpine
```

Запустить в контейнере команду `ls`:

```
ls /
```

Ожидаемый результат:

```
bin    etc    lib    mnt    root   sbin   sys    usr
dev    home   media  proc   run    srv    tmp    var
```

## Автоматическое удаление контейнера после использования

Каждый раз, когда создается новый контейнер, он занимает некоторое пространство, хотя обычно оно минимально.
Чтобы узнать, сколько места занимают контейнеры, запустить команду `docker container ls -as`.

```bash
CONTAINER ID   IMAGE         COMMAND                  CREATED             STATUS                         PORTS     NAMES                 SIZE
2d8c22573d76   alpine        "/bin/sh"                3 minutes ago       Exited (130) 4 seconds ago               friendly_williams     3B (virtual 7.38MB)
```
Здесь видно, что сам образ alpine занимает 7.38MB, а сам контейнер — 3B. Когда манипулируем с файлами в контейнере, размер контейнера увеличится.

Если создается множество новых контейнеров, можно указать Docker удалить контейнер после остановки с помощью опции `--rm`:
`docker run --rm -it alpine`

Это приведет к удалению контейнера сразу после его остановки.

## Очистка контейнеров, которые больше не используются

Контейнеры по-прежнему сохраняются, даже если они остановлены.
Чтобы удалить их с сервера, использовать команду `docker rm`.
`docker rm` может принимать либо `CONTAINER ID`, либо `NAME`, как показано выше.

Удалить контейнер hello-world:

```
 docker container ls -a
```

Ожидаемый результат:

```
CONTAINER ID   IMAGE         COMMAND                  CREATED             STATUS                         PORTS     NAMES
fe8d001b3ca6   hello-world   "/hello"                 About an hour ago   Exited (0) About an hour ago             frosty_shockley
```

Удаление контейнера:

```
docker container rm frosty_shockley
```

Указанное имя или идентификатор возвращается в виде успешного выполнения команды:

```
frosty_shockley
```
Контейнер теперь отсутствует в списке после выполнения команды  `ls -a`.

> :bulb: **Совет:** Как и в случае с Git, для ссылки на него можно использовать любую уникальную часть идентификатора контейнера.

### Удаление изображений

Удален экземпляр контейнера выше, но не сам образ hello-world. Удалить образ.

Прежде всего, проверьте все образы, которые загружены в облачную среду:

```
docker image ls
```

Ожидаемый результат:

```
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
alpine        latest    05455a08881e   22 hours ago   7.38MB
hello-world   latest    d2c94e258dcb   9 months ago   13.3kB
```
Здесь можно увидеть загруженные образы, а также их размер.
Чтобы удалить образ hello-world, используйте команду `docker image rm` вместе с идентификатором образа docker.

```
docker image rm d2c94e258dcb
```
Ожидаемый результат:

```
Error response from daemon: conflict: unable to delete d2c94e258dcb (must be forced) - image is being used by stopped container a59b7f0cdf73

```
Требуется использовать ключ `--force` для удаления образов, которые используются в виртуальном окружении.

```
docker rmi --force d2c94e258dcb
```

Ожидаемый результат:

```
Untagged: hello-world:latest
Untagged: hello-world@sha256:c5515758d4c5e1e838e9cd307f6c6a0d620b5e07e6f927b07d05f6d12a1ac8d7
Deleted: sha256:48b5124b2768d2b917edcb640435044a97967015485e812545546cbed5cf0233
Deleted: sha256:98c944e98de8d35097100ff70a31083ec57704be0991a92c51700465e4544d08
```

### Очистка

При создании, запуске и восстановлении образов загружается и сохраняется множество слоев. Эти слои не будут удалены, поскольку докер использует очень консервативный подход к очистке.

Docker предоставляет команду `prune`, забираем все зависшие контейнеры/изображения/сети/тома.

- `docker container prune`
- `docker image prune`
- `docker network prune`
- `docker volume prune`

Команда docker image prune позволяет очистить неиспользуемые изображения. По умолчанию `docker image prune` очищает только висячие изображения. Зависшиы образ — это компонент, который не помечен тегами и на который не ссылается какой-либо контейнер. 

Если требуется полная очистка, выполнить `docker system prune`.

