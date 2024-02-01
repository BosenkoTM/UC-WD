# Практическое задание 8. Многоэтапные сборки. Мини контейнер Go-приложения.

## Задача: создать мини контейнер go-приложения.

В [multi-stage-build/hello.go](https://github.com/BosenkoTM/UC-WD/tree/main/docker-compose/labs/multi-stage-build) создано приложение go, которое выводит `hello world`.

Использовать «базовый образ», в котором есть go!

[Dockerfile](multi-stage-build/Dockerfile) уже создан для в той же папке.

## Упражнение 8.1

- Собрать образ с помощью `docker build`.
- Запустить его с помощью docker run.
- Получить в терминале сообщение `Hello world!`.

## Использование многоэтапных сборок

Созданный образ содержит и компилятор, и скомпилированный двоичный файл — а это слишком много: двоичный файл нужен только для запуска  приложения.

Используя многоэтапные сборки, можно отделить этап сборки (компиляцию) от образа, который требуется использовать.

## Упражнение 8.2

- выполнить `docker image ls`.

- Возможно ли уменьшение размера контейнера? Компилятор нужен только во время сборки, поскольку go — статически компилируемый язык.

- См. «Dockerfile» ниже, он состоит из двух «этапов сборки», причем последний этап использует скомпилированный артефакт (двоичный файл) из первого:


```Dockerfile
# build stage
FROM golang:1.19 as builder
WORKDIR /app
COPY . /app
RUN go mod download && go mod verify
RUN cd /app && go build -o goapp

# final stage
FROM alpine
WORKDIR /app
COPY --from=builder /app/goapp /app/
ENTRYPOINT ./goapp
```
- Заменить исходный файл Dockerfile на приведенный выше и создать его с другим тегом.

- При запуске должен напечатать в терминале `Hello world!`.

- Проверить размер контейнера с помощью `docker image ls`.

- Сравните размер двух образов. Последний образ должен быть намного меньше, поскольку он содержит только go-приложение, использует `alpine` в качестве `базового образа`, а не весь набор инструментов `golang`.

```
REPOSITORY            TAG           IMAGE ID       CREATED          SIZE
hello                 golang        5311178b692a   23 seconds ago   805MB
hello                 multi-stage   ba46dc3143ca   2 minutes ago    7.53MB
```

Подробнее здесь: [Использовать многоэтапные сборки ](https://docs.docker.com/develop/develop-images/multistage-build/).

## Индивидуальное задание
Поскольку `go` — статически компилируемый язык,  использовать `Scratch` в качестве «базового образа».
Образ `scratch` — это просто пустая файловая система.

Подсказка: образ «с нуля» не имеет оболочки, поэтому в Dockerfile  _также_ необходимо изменить `ENTRYPOINT` с `shell form` на `exec form`.
См.: `ENTRYPOINT` в [справке по Dockerfile] (https://docs.docker.com/engine/reference/builder/).

После сборки нового Dockerfile проверить размер образов.
Новый образ должен быть меньше, чем образ на основе Alpine!

```Dockerfile
FROM golang:1.19 as builder
WORKDIR /app
COPY . /app
RUN go mod download && go mod verify
RUN cd /app && go build -o goapp
FROM scratch
WORKDIR /app
COPY --from=builder /app/goapp /app/
ENTRYPOINT ["./goapp"]
```
