# Docker for Beginners - Linux
## Задача 0: Предпосылки
Клонируйте репозиторий GitHub лаборатории
```
git clone https://github.com/dockersamples/linux_tweet_app
```
![alt text](image.png)

## Задача 1: Запуск нескольких простых контейнеров Docker
Запуск одной задачи в контейнере Alpine Linux
```
 docker container run alpine hostname
```
![alt text](image-1.png)

Перечислите все контейнеры.
```
docker container ls --all
```
![alt text](image-2.png)

Запустите интерактивный контейнер Ubuntu
```
docker container run --interactive --tty --rm ubuntu bash
```
Выполните следующие команды в контейнере.
```
ls /
ps aux
cat /etc/issue
 exit
```
![alt text](image-3.png)

Запустить фоновый контейнер MySQL
```
 docker container run \
 --detach \
 --name mydb \
 -e MYSQL_ROOT_PASSWORD=my-secret-pw \
 mysql:latest
 ```
![alt text](image-4.png)

 Перечислите работающие контейнеры.
 ```
docker container ls
```
![alt text](image-5.png)
Вы можете проверить, что происходит в ваших контейнерах, используя пару встроенных команд Docker:
```
 docker container logs mydb
```
![alt text](image-6.png)

Рассмотрим процессы, протекающие внутри контейнера.
```
docker container top mydb
```
![alt text](image-7.png)

Выведите версию MySQL
```
 docker exec -it mydb \
 mysql --user=root --password=$MYSQL_ROOT_PASSWORD --version
```
![alt text](image-8.png)

```
docker exec -it mydb sh
mysql --user=root --password=$MYSQL_ROOT_PASSWORD --version
exit
```
![alt text](image-9.png)

## Задача 2: Упаковка и запуск пользовательского приложения с использованием Docke
Создайте простое изображение веб-сайта
```
cd ./linux_tweet_app
cat Dockerfile
```
![alt text](image-10.png)

```
$env:DOCKERID = "adilyabatyrbekova"
echo $env:DOCKERID
```
![alt text](image-11.png)

```
docker image build --tag "$env:DOCKERID/linux_tweet_app:1.0" .
```
![alt text](image-13.png)

```
 docker container run --detach --publish 80:80 --name linux_tweet_app --mount type=bind,source="$(pwd)",target=/usr/share/nginx/html "$env:DOCKERID/linux_tweet_app:1.0"
docker rm --force linux_tweet_app
 ```
![alt text](image-17.png)

 ## Задача 3: Изменить работающий веб-сайт
Запустите наше веб-приложение с помощью привязки монтирования
```
docker container run --detach --publish 80:80 --name linux_tweet_app --mount type=bind,source="$(pwd)",target=/usr/share/nginx/html "$env:DOCKERID/linux_tweet_app:1.0"
```
![alt text](image-18.png)

Изменить работающий веб-сайт
```
docker rm --force linux_tweet_app
docker container run `
--detach `
--publish 80:80 `
--name linux_tweet_app `
"$env:DOCKERID/linux_tweet_app:1.0"
docker rm --force linux_tweet_app
```
![alt text](image-19.png)    

Обновить изображение
```
docker image build --tag "$env:DOCKERID/linux_tweet_app:2.0" .
docker image ls
```
![alt text](image-20.png)

Загрузите свои образы в Docker Hub
```
docker image ls -f reference="$DOCKERID/*"
docker login
docker image push $DOCKERID/linux_tweet_app:1.0
docker image push $DOCKERID/linux_tweet_app:2.0
```
![alt text](image-21.png)
![alt text](image-22.png)

# Application Containerization and Microservice Orchestration
### Установка сцены
```
git clone https://github.com/ibnesayeed/linkextractor.git
cd linkextractor
git checkout demo
```
![alt text](image-23.png)
![alt text](image-24.png)

## Шаг 0: Базовый скрипт извлечения ссылок
```
git checkout step0
tree
```
![alt text](image-25.png)

```
cat linkextractor.py
./linkextractor.py http://example.com/
ls -l linkextractor.py
python3 linkextractor.py
```
![alt text](image-26.png)

## Шаг 1: Контейнеризованный скрипт извлечения ссылок
```
git ls-tree -r step1 --name-only
cat Dockerfile
docker image build -t linkextractor:step1 .
docker image ls
docker container run -it --rm linkextractor:step1 http://example.com/
```
![alt text](image-27.png)
![alt text](image-28.png)
![alt text](image-29.png)
![alt text](image-30.png)
![alt text](image-31.png)

## Шаг 2: Модуль извлечения ссылок с полным URI и текстом привязки
```
git checkout step2
git ls-tree -r step2 --name-only
cat linkextractor.py
docker image build -t linkextractor:step2 .
docker image ls
docker container run -it --rm linkextractor:step2 https://training.play-with-docker.com/
```
![alt text](image-32.png)
![alt text](image-33.png)
![alt text](image-34.png)
![alt text](image-35.png)
![alt text](image-36.png)


## Шаг 3: API-сервис извлечения ссылок
```
git checkout step3
git ls-tree -r step3 --name-only
cat Dockerfile
cat main.py
docker image build -t linkextractor:step3 .
docker container run -d -p 5000:5000 --name=linkextractor linkextractor:step3
docker container ls
docker container rm -f linkextractor
```
![alt text](image-37.png)
![alt text](image-38.png)
![alt text](image-39.png)
![alt text](image-40.png)
![alt text](image-41.png)
![alt text](image-42.png)

## Шаг 4: API извлечения ссылок и службы веб-интерфейса
```
git checkout step4
git ls-tree -r step4 --name-only
cat docker-compose.yml
cat www/index.php
docker-compose up -d --build
docker container ls
git reset --hard
docker-compose down
```
![alt text](image-43.png)
![alt text](image-44.png)
![alt text](image-45.png)
![alt text](image-46.png)
![alt text](image-47.png)
![alt text](image-48.png)
![alt text](image-49.png)

## Шаг 5: Служба Redis для кэширования
```
git checkout step5
git ls-tree -r step5 --name-only
cat www/Dockerfile
cat docker-compose.yml
docker-compose up -d --build
docker-compose exec redis redis-cli monitor
git reset --hard
docker-compose down
```
![alt text](image-50.png)
![alt text](image-51.png)
![alt text](image-52.png)
![alt text](image-53.png)
![alt text](image-54.png)
![alt text](image-55.png)
![alt text](image-56.png)

## Шаг 6: замена службы API Python на Ruby
```
git checkout step6
git ls-tree -r step6 --name-only
cat api/linkextractor.rbA
cat api/Dockerfile
docker-compose up -d --build
docker-compose down
```
![alt text](image-57.png)
![alt text](image-58.png)
![alt text](image-59.png)
![alt text](image-60.png)
![alt text](image-61.png)
![alt text](image-62.png)

# Deploying a Multi-Service App in Docker Swarm Mode
## Инициируйте свой рой
```
docker swarm init --advertise-addr (Get-NetIPAddress -AddressFamily IPv4 | Where-Object { $_.InterfaceAlias -like "*Ethernet*" }).IPAddress
```
![alt text](image-63.png)

## Клонировать приложение для голосования
```
git clone https://github.com/docker/example-voting-app
```
![alt text](image-64.png)

## Развернуть стек
```
docker stack deploy --compose-file=docker-stack.yml voting_stack
docker stack ls
```

![alt text](image-65.png)
![alt text](image-66.png)