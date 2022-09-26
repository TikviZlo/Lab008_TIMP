
## 1. Устанавливаем docker:

```
$ sudo apt install -y docker.io
```

## 2. Пишем примеры (examples и src).

## 3. Пишем DockerFile:

```
FROM ubuntu:20.04

RUN apt update
RUN apt install -yy gcc g++ cmake

COPY . app/
WORKDIR app/exec/

RUN cmake -H. -B_build
RUN cmake --build _build

WORKDIR _build/

VOLUME ["/app/logs"]

ENTRYPOINT ./example1 && ./example2
```

## 4. Собираем образ контейнера:

```
$ docker build .
```
