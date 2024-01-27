# Практическое задание 2. Запуск контейнера из образа.

## Цель


- Запустить [Alpine Linux](http://www.alpinelinux.org/) контейнер (облегчённый дистрибутив Linux) в вашей системе и проверьте команду `docker run`.

## Введение

Чтобы начать работу с первым контейнером из образа, сначала получите образ Alpine Linux, облегченный дистрибутив Linux, а затем изучите различные команды для взаимодействия с ним.

## Упражнение 2.1

### Задачи

- Извлеките образ Alpine Linux.
- Перечислите все образы в системе.
— Запустите Docker-контейнер на основе образа Alpine.
- Исследуйте различные команды внутри контейнера.
- Изучите имена и идентификаторы контейнеров.

Запустить в терминале следующие команды:
* `docker pull alpine`

Команда `pull` извлекает **образ** alpine из **реестра Docker** и сохраняет его в системе. Используйте команду `docker image ls`, чтобы просмотреть список всех изображений в вашей системе.

* `docker image ls`

Ожидаемый результат (ваш список образов будет выглядеть иначе):

``` bash
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
alpine        latest    05455a08881e   21 hours ago   7.38MB
hello-world   latest    d2c94e258dcb   9 months ago   13.3kB
```

## 1.1 запуск докера

Запустить **контейнер** Docker на основе этого образа.

* `docker run alpine ls -l`

Ожидаемый результат:


```bash
total 56
drwxr-xr-x    2 root     root          4096 Jan 26 17:53 bin
drwxr-xr-x    5 root     root           340 Jan 27 21:07 dev
drwxr-xr-x    1 root     root          4096 Jan 27 21:07 etc
drwxr-xr-x    2 root     root          4096 Jan 26 17:53 home
......
......
```
Когда запускаем `docker run alpine`, используется ключ (`ls -l`), поэтому Docker запускает указанную команду, и в результате видим каталоги с правами доступа.

Запустить команду:

* `docker run alpine echo "привет от Alpine"`

Ожидаемый результат:

``` bash
привет от Alpine
```

<details>
<summary>More Details</summary>
In this case, the Docker client ran the `echo` command in our alpine container and then exited it. If you've noticed, all of that happened pretty quickly. Imagine booting up a virtual machine, running a command and then killing it. Now you know why they say containers are fast!

В этом случае клиент Docker выполнил команду `echo` в контейнере Alpine, а затем вышел из него. Если заметили, все это произошло довольно быстро. Представьте, что загружаете виртуальную машину, запускаете команду, а затем удаляете ее. Контейнеры — это быстро!
</details>

Запустить команду:

* `docker run alpine /bin/sh`

Эти интерактивные оболочки завершатся после выполнения любых команд сценария, если только они не будут запущены в интерактивном терминале - поэтому, чтобы этот пример не завершался, необходимо добавить параметры `i` и `t`. 

> :bulb: Флаги `-it` являются сокращением от `-i -t`, которые опять же являются короткими формами `--interactive` (держать STDIN открытым) и `--tty` (изолировать терминал). 

* `docker run -it alpine /bin/sh`

Вы находитесь внутри оболочки контейнера и можете опробовать несколько команд, таких как `ls -l`, `uname -a` и другие.

* Выйдите из контейнера, используя команду `exit`.

Команда `docker ps` показывает все контейнеры, которые в данный момент работают.

The `docker ps` command shows you all containers that are currently running.

* `docker ps`

Ожидаемый результат:

```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```
Обратите внимание, что нет работающих контейнеров. После выхода из оболочки `exit`, основной процесс (`/bin/sh`) остановился. Ни один контейнер не запущен, видим пустую строку. Перечислим все контейнеры, как остановленные, так и запущенные.

* `docker ps -a`

Ожидаемый результат:

```
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                            PORTS     NAMES
9ed317294076   alpine        "/bin/sh"                5 minutes ago    Exited (130) About a minute ago             nervous_dhawan
4a2a3622537e   alpine        "/bin/sh"                8 minutes ago    Exited (0) 8 minutes ago                    blissful_montalcini
3e365f16195c   alpine        "echo 'привет от Alp…"   12 minutes ago   Exited (0) 12 minutes ago                   crazy_sutherland
5892a1ba8f41   alpine        "ls -l"                  17 minutes ago   Exited (0) 17 minutes ago                   hungry_volhard
fe8d001b3ca6   hello-world   "/hello"                 30 minutes ago   Exited (0) 30 minutes ago                   frosty_shockley
43e2068450e9   hello-world   "/hello"                 35 minutes ago   Exited (0) 35 minutes ago                   reverent_vaughan
a59b7f0cdf73   hello-world   "/hello"                 45 minutes ago   Exited (0) 45 minutes ago                   cool_mcnulty
```

Выше вы видите список всех контейнеров, которые вы запускали. Обратите внимание, что в столбце `STATUS` показано, что эти контейнеры завершились несколько минут назад. 

## Именование контейнера

Посмотрим еще раз на вывод `docker ps -a`:

* `docker ps -a`

Все контейнеры имеют **ID** и **name**.  

И идентификатор, и имя генерируются каждый раз, когда создается новый контейнер со случайным начальным значением для обеспечения уникальности.

> :bulb: Совет: Если хотите присвоить контейнеру определенное имя, можно использовать опцию `--name`. Это упростит обращение к контейнеру в дальнейшем. Чтобы узнать больше о `run`, используйте `docker run --help`.

