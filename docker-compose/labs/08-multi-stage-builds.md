# Практическое задание 8. Многоэтапные сборки. Мини контейнер Go-приложения.

## Задача: создать мини контейнер go-приложения.

В [multi-stage-build/hello.go](https://github.com/BosenkoTM/UC-WD/tree/main/docker-compose/labs/multi-stage-build) создано приложение go, которое выводит `hello world`.

Использовать «базовый образ», в котором есть go!

[Dockerfile](multi-stage-build/Dockerfile) уже создан для в той же папке.

## Exercise

- Try building the image with `docker build`
- Try to run it with `docker run`.
- You should see "Hello world!" printed to your terminal.

## Using Multi Stage Builds

The image we built has both the compiler and the compiled binary - which is too much: we only need the binary to run our application.

By utilizing multi-stage builds, we can separate the build stage (compiling) from the image we actually want to ship.

## Exercise

- try `docker image ls`.

- Could we make it smaller? We only need the compiler on build-time, since go is a statically compiled language.

- See the `Dockerfile` below, it has two `build stages`, wherein the latter stage is using the compiled artifact (the binary) from the first:

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

- Replace the original `Dockerfile` with the one above, and try building it with a different tag.

- When you run it it should still print "Hello world!" to your terminal.

- Try inspecting the size with `docker image ls`.

- Compare the size of the two images. The latter image should be much smaller, since it's just containing the go-application using `alpine` as the `base image`, and not the entire `golang`-suite of tools.

You can read more about this on: [Use multi-stage builds - docs.docker.com](https://docs.docker.com/develop/develop-images/multistage-build/)

You should see a great reduction in size, like in the example below:

```
REPOSITORY            TAG           IMAGE ID       CREATED          SIZE
hello                 golang        5311178b692a   23 seconds ago   805MB
hello                 multi-stage   ba46dc3143ca   2 minutes ago    7.53MB
```

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
