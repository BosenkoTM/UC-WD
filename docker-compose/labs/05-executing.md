# Практическое задание 5. Запуск и выполнение процессов в контейнере.

Если требуется проверить работающий контейнер, при этом не завершать работающий процесс, можно запустить другой процесс внутри контейнера с помощью `exec`.


## Цель

Работа с файлом в работающем контейнере, запустив параллельную задачу.

### Ход работы

— Развернуть новый контейнер NGINX: `docker run -d -p 8000:80 nginx`
— Проверить веб-страницу, убедиться, что NGINX настроен правильно.

Войдите в новый контейнер, выполнив оболочку bash внутри контейнера:

```
docker exec -it CONTAINERNAME bash
```
> :bulb: обратите внимание, что CONTAINERNAME — это имя контейнера NGINX.

Внутри отредактировать страницу index.html с помощью текстового редактора `cli` [Nano](https://www.nano-editor.org/).

> :bulb: Из [описания DockerHub](https://hub.docker.com/_/nginx) стандартное размещение HTML-страниц под управлениемт NGINX, находится в /usr/share/nginx/html.

— установите nano в контейнер: `apt-get update && apt-get install -y nano`
- Отредактируйте `index` html-страницу: `nano /usr/share/nginx/html/index.html`:

```
<!DOCTYPE html>

<html lang="en">

<head>

<meta charset="UTF-8">

<title> Example of a simple html page</title>

</head>

<body>

Example of a simple page - to view the code, press ctrl + U
</body>

</html>
```
    
- Сохраните и выйдите из nano, нажав: «CTRL + O» и «enter», чтобы сохранить, и «CTRL + X», чтобы выйти из `Nano`.
– Повторно зайти на страницу, проверить изменения, внесенные на странице.

## Задание
