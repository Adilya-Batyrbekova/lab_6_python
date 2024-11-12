### Клонирование репозитория с GitHub лаборатории
``` git clone https://github.com/dockersamples/linux_tweet_app ```

### Запуск одной задачи в контейнере Alpine Linux
``` docker container run alpine hostname ```
![alt text](image.png)

``` docker container ls --all ``` 
![alt text](image-1.png)

### Запустите интерактивный контейнер Ubuntu
Запустите Docker-контейнер и получите доступ к его оболочке.
```  docker container run --interactive --tty --rm ubuntu bash ```
![alt text](image-2.png)

### Запустить фоновый контейнер MySQL
Запустите новый контейнер MySQL с помощью следующей команды.
