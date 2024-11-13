# Your First Linux Containers
## 1. Запуск первого контейнера
``` docker container run hello-world ```
![alt text](image.png)

### 1.1 Образы Docker
``` docker image pull alpine ```
![alt text](image-1.png)

Просмотреть список всех образов в системе.
``` docker image ls ```
![alt text](image-2.png)

### 1.2 Запуск Docker-контейнера
``` docker container run alpine ls -l ```
![alt text](image-3.png)

Тестирование различных команд
Список команд
```
docker container run alpine echo "hello from alpine"
docker container run alpine /bin/sh
docker container run -it alpine /bin/sh
docker container ls
docker container ls -a
```
![alt text](image-4.png)

### 1.3 Изоляция контейнера
``` docker container run -it alpine /bin/ash ```
![alt text](image-5.png)

Демонстрация работы изоляции
``` docker container run alpine ls ```
![alt text](image-6.png)

``` docker container ls -a ```
![alt text](image-7.png)

# Customizing Docker Images:
## Создание образа из контейнера
``` docker container run -ti ubuntu bash ```

Установим пакет figlet в этот контейнер
```
apt-get update
apt-get install -y figlet
figlet "hello docker"
```
![alt text](image-8.png)

``` docker container ls -a ```
![alt text](image-9.png)

Выполнение списка команд 
``` 
docker container commit bb5a
docker image ls
docker image tag 210a ourfiglet
docker image ls
```
![alt text](image-10.png)

Теперь запустим контейнер на основе недавно созданного образа ourfiglet :
``` docker container run ourfiglet figlet hello ```
![alt text](image-11.png)

## Создание образа с помощью Dockerfile
Создадим образ из Dockerfile и назовем его hello:v0.1
``` docker image build -t hello:v0.1 . ```
 
Затем мы запускаем контейнер, чтобы проверить правильность работы наших приложений:
``` docker container run hello:v0.1 ``` 
![alt text](image-12.png)

## Слои изображения
``` docker image history <image ID> ```
![alt text](image-13.png)

``` 
echo "console.log(\"this is v0.2\");" >> index.js
docker image build -t hello:v0.2 .
```
![alt text](image-14.png)

## Проверка изображения
``` docker image inspect alpine ```
![alt text](image-15.png)


Получим список слоев:
``` docker image inspect --format "{{ json .RootFS.Layers }}" alpine ```
![alt text](image-16.png)

Теперь посмотрим на наше пользовательское изображение Hello.
![alt text](image-17.png)

# Deploy and Managing Multiple Containers
## Инициализируйте свой рой
Инициализация Docker Swarm Mode
``` docker swarm init --advertise-addr $(hostname -i) ```
![alt text](image-18.png)

Показать участников Swarm
``` docker node ls ```
![alt text](image-19.png)

## Клонировать приложение для голосования
```
git clone https://github.com/docker/example-voting-app
cd example-voting-app 
```
![alt text](image-20.png)

### Развернуть стек
``` cat docker-stack.yml ```
![alt text](image-21.png)

```
docker stack deploy --compose-file=docker-stack.yml voting_stack
docker stack ls
docker stack services voting_stack
docker service ps voting_stack_vote
```
![alt text](image-22.png)
### Масштабирование приложения
```
docker service scale voting_stack_vote=5
ocker stack services voting_stack
```
![alt text](image-23.png)
