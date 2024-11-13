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
