# Практическое задание 6. Докер-том.

> _Hint: Задание охватывает только тома Docker для Linux._

Хранение данных лучше выполнять вне контейнера. Идея заключаетсяв том, чтобы запускать, останавливать и удалять контейнеры без потери данных.

Существует два способа монтирования данных из вашего контейнера: `bind mounts` и `volumes`.

**A bind mount**. Он принимает путь хоста, например `/data`, и монтирует его внутри вашего контейнера, например. `/opt/app/data`. **A bind mount** хорош тем, что  позволяет подключаться напрямую к файловой системе хоста. Недостатком является то, что нужно указывать путь во время выполнения контейнера, а путь для монтирования может отличаться от хоста к хосту. При использовании привязки вам также придется выполнять резервное копирование, миграцию и т. д. с помощью инструмента вне экосистемы `Docker`.

**Том докера** — это место, где используется том с именем или без имени для хранения внешних данных. Обычно для этого используется драйвер тома, но можно получить путь подключения к хосту, используя драйвер локального тома по умолчанию.

## Bind mounts

Рассмотрим контейнер [Nginx](https://hub.docker.com/_/nginx/).
Сам по себе сервер бесполезен, если он не может получить доступ к нашему веб-контенту на хосте.

Создать сопоставление между хост-системой и контейнером с помощью команды `-v`:

```bash
docker run --name some-nginx -v /some/content:/usr/share/nginx/html:ro -d nginx
```
Это сопоставит все файлы, находящиеся в папке `/some/content` на хосте, с `/usr/share/nginx/html` в контейнере.

> Атрибут `:ro` делает том хоста доступным только для чтения, гарантируя, что контейнер не сможет редактировать файлы на хосте.

## Ход работы

- `git clone` этот репозиторий.
- Перейдите в каталог `labs/volumes/`, который содержит файл, который требуется использовать сервером Nginx: `index.html`.
- Изменить `/some/content` на локальный путь, это должен быть абсолютный путь, начиная с корня файловой системы (который в Linux — `/`). Использовать команду `pwd` (вывод пути рабочего каталога).
- Запустить контейнер с установленной привязкой к каталогу `labs/volume`. Это даст  работающий сервер `Nginx`, обслуживающий статические файлы... _Но на каком порту?_
- Запусть команду `docker ps`, чтобы узнать, есть ли какие-либо порты, перенаправленные с хоста.
- Разместить сайт на порт 8000.

<details>
<summary>Как это сделать?</summary>
  
Параметр `-p 8000:80` сопоставит порт 80 в контейнере с портом 8000 на хосте.

</details>

- Перейти на хост или IP-адрес в браузере, указав порт 8000.
- Остановите контейнер с помощью `docker stop <container_name>`.

## Volumes

Том — это объект внутри `Docker`, существует три способа их создания:

- Явноу создание, с помощью команды `docker volume create <volume_name>`.
- Создание именованного тома во время создания контейнера с помощью `docker container run -d -v DataVolume:/opt/app/data nginx`.
- Создание анонимного тома во время создания контейнера с помощью `docker container run -d -v /opt/app/data nginx`.

Создать том данных под названием `data:

```bash
docker volume create data
```
Docker создает том и выведет имя созданного тома.

Запустить `docker volume ls`:

```outputs
DRIVER              VOLUME NAME
local               data
```
В отличие от монтирования с привязкой, вы не указываете, где на хосте хранятся данные.

В API тома, как и во всех других API Docker, есть команда `inspect`, предоставляющая подробную информацию на низком уровне.

— запустите `docker volume inspect data`, чтобы увидеть, где данные хранятся на хосте.

```json
[
    {
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/data/_data",
        "Name": "data",
        "Options": {},
        "Scope": "local"
    }
]
```
Том данных смонтирован в `/var/lib/docker/volumes/data/_data` на хосте.


> **Примечание** Здесь не рассматриваются драйверы. Для получения дополнительной информации посмотрите [example](https://docs.docker.com/engine/admin/volumes/volumes/#use-a-volume-driver).

Использовать этот том данных во всех контейнерах. Подключить его к серверу `nginx` с помощью команды `docker container run --rm --name www -d -p 8080:80 -v data:/usr/share/nginx/html nginx`.

> **Note:** If the volume refer to is empty and we provide the path to a directory that contains data in the base image, that data will be copied into the volume.

Try now to look at the data stored in `/var/lib/docker/volumes/data/_data` on the host:

```
sudo ls /var/lib/docker/volumes/data/_data/
```

Expected output:

```
50x.html  index.html
```

Those two files comes from the Nginx image and is the standard files the webserver has.

### Attaching multiple containers to a volume

Multiple containers can attach to the same volume with data. 

Docker doesn't handle any file locking, so applications must account for the file locking themselves.


Let's try to go in and make a new html page for nginx to serve. 

We do this by making a new ubuntu container that has the `data` volume attached to `/tmp`, and thereafter create a new html file with the `echo` command:

Start the container:

```
docker run -it --rm -v data:/tmp ubuntu bash
```

In the container run:

```
echo "<html><h1>hello world</h1></html>" > /tmp/hello.html
```

Verify the file was created by running in the container:

```
ls /tmp
```

Expected output:

```
hello.html  50x.html  index.html
```

Head over to your newly created webpage at: `http://<IP>:8080/hello.html`

## cleanup

Exit out of your ubuntu server and execute a `docker stop www` to stop the nginx container.

Run a `docker ps` to make sure that no other containers are running.

```
docker ps
```

Expected output:

```
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS                                                          NAMES
```

The data volume is still present, and will be there until you remove it with a `docker volume rm data` or make a general cleanup of all the unused volumes by running `docker volume prune`.

## Tips and tricks

As you have seen, the `-v` flag can both create a bind mount or name a volume depending on the syntax. If the first argument begins with a / or ~/ you're creating a bind mount. Remove that, and you're naming the volume. For example:

- `-v /path:/path/in/container` mounts the host directory, `/path` at the `/path/in/container`
- `-v path:/path/in/container` creates a volume named path with no relationship to the host.

### Sharing data

If you want to share volumes or bind mount between two containers, then use the `--volumes-from` option for the second container. The parameter maps the mapped volumes from the source container to the container being launched.

## More advanced docker commands

Before you go on, use the [Docker command line interface](https://docs.docker.com/engine/reference/commandline/cli/) documentation to try a few more commands:

- While your detached container is running, use the `docker ps` command to see what silly name Docker gave your container. **This is one command you're going to use often!**
- While your detached container is still running, look at its logs. Try following its logs and refreshing your browser.
- Stop your detached container, and confirm that it is stopped with the `ps` command.
- Start it again, wait 10 seconds for it to fire up, and stop it again.
- Then delete that container from your system.

> **NOTE:** When running most docker commands, you only need to specify the first few characters of a container's ID. For example, if a container has the ID `df4fd19392ba`, you can stop it with `docker stop df4`. You can also use the silly names Docker provides containers by default, such as `boring_bardeen`.

If you want to read more, I recommend [Digital Oceans](https://www.digitalocean.com/community/tutorials/how-to-share-data-between-docker-containers) guides to sharing data through containers, as well as Dockers own article about [volumes](https://docs.docker.com/engine/admin/volumes).

## summary

Now you have tried to bind volumes to a container to connect the host to the container.
