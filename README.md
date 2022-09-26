
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
$ docker build -t logger .
```

```
[+] Building 12.0s (13/13) FINISHED                                                                                                   
 => [internal] load build definition from Dockerfile                                                                             0.0s
 => => transferring dockerfile: 38B                                                                                              0.0s 
 => [internal] load .dockerignore                                                                                                0.0s 
 => => transferring context: 2B                                                                                                  0.0s 
 => [internal] load metadata for docker.io/library/ubuntu:20.04                                                                 11.9s 
 => [1/8] FROM docker.io/library/ubuntu:20.04@sha256:35ab2bf57814e9ff49e365efd5a5935b6915eede5c7f8581e9e1b85e0eecbe16            0.0s
 => [internal] load build context                                                                                                0.0s 
 => => transferring context: 6.10kB                                                                                              0.0s 
 => CACHED [2/8] RUN apt update                                                                                                  0.0s 
 => CACHED [3/8] RUN apt install -yy gcc g++ cmake                                                                               0.0s 
 => CACHED [4/8] COPY . app/                                                                                                     0.0s 
 => CACHED [5/8] WORKDIR app/examples/                                                                                           0.0s 
 => CACHED [6/8] RUN cmake -H. -B_build                                                                                          0.0s 
 => CACHED [7/8] RUN cmake --build _build                                                                                        0.0s 
 => CACHED [8/8] WORKDIR _build/                                                                                                 0.0s 
 => exporting to image                                                                                                           0.0s 
 => => exporting layers                                                                                                          0.0s 
 => => writing image sha256:00cda13fd3b04ad585a04ffdee621684fe0df64bfadb64d2be8c14bdb63aeb3a                                     0.0s 
 => => naming toра docker.io/library/logger 
```

## 5. Запускаем контейнер:

```
$ docker run --name test -v "E:/app/logs" logger
```

```
hello1
End of example1 program!!!                                                                                                           
End of example2 program!!
```

## 6. Копируем файл логов из контейнера и проверяем его содержимое.

```
$ docker cp test:/app/logs/log.txt .
```

```
hello2
```

Программа сработала корректно.
