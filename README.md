# fedosovdv_microservices
fedosovdv microservices repository

## ДЗ-18 (logging-1)
- Подготовлено окружение
- Логирование Docker-контейнеров
- Сбор неструктурированных логов
- Визуализация логов
- Сбор структурированных логов
- Распределенный трейсинг

## ДЗ-17 (monitoring-1)
- Развернут Prometheus в докере
- Prometheus добавлен в docker-compose.yml, настроен для мониторинга конечных служб
- Проверены Healthchecks (остановкой контейнера)
- Установлен Exporters, проверено возрастание нагрузки при имитации таковой

## ДЗ-16 (gitlab-ci-1)
- создана ВМ через cli yandex
- настроен GitLab(получен пароль root; закрыта регистрация)
- добавлен Reddit в проект
- на локальную машину (ввиду нехватки памяти для тестов) поставлен раннер
- добавлена зависимость от тегов для production и stage
- добавлены динамические окружения

## ДЗ-15 (docker-4)
**1. Сетями в Docker**
- Рассмотрены типы сетей в Docker none, host, bridge

**2. docker-compose**
- параметризирован **docker-compose.yml**
- прописаны переменные в **.env**

Имя проекта по умолчанию - имя каталога где лежит  **docker-compose.yml**
поменять мохно `docker-compose -p [имя_проекта] up -d` или указав переменной окружения **COMPOSE_PROJECT_NAME**


еще вариант запуска с переменными из файла : ```
env $(cat <env-file> | xargs)  docker-compose up -d```


## ДЗ-14 (docker-3)
1.Разбивка приложения reddit на отдельные образы
1.1 починка образ post-py (add MarkupSafe==1.1.1 in post-py/requirements.txt)
2.Оптимизация образов по размеру  - замена образа ubuntu на ruby на базе alpine
3.Работа с Docker volume


### Для запуска необходимо:
собрать:
```
docker build -t post:1.0 ./post-py
docker build -t comment:1.0 ./comment
docker build -t ui:2.0 ./ui
```
добавить network,volume
```
docker network create reddit
docker volume create reddit_db
```
запустить:
```
docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db -v reddit_db:/data/db mongo:latest
docker run -d --network=reddit --network-alias=post post:1.0
docker run -d --network=reddit --network-alias=comment comment:1.0
docker run -d --network=reddit -p 9292:9292 ui:2.0
```

## ДЗ-12 (docker-2)

* Создание docker host на YC
```
yc compute instance create \
  --name docker-host \
  --zone ru-central1-a \
  --network-interface subnet-name=default-ru-central1-a,nat-ip-version=ipv4 \
  --create-boot-disk image-folder-id=standard-images,image-family=ubuntu-1804-lts,size=15 \
  --ssh-key ~/.ssh/appuser.pub
docker-machine create \
  --driver generic \
  --generic-ip-address=<!!!_IP_ADDRESS_!!!> \
  --generic-ssh-user yc-user \
  --generic-ssh-key ~/.ssh/appuser \
  docker-host
eval $(docker-machine env docker-host)
```
* Создание образа

```
cd docker-monolith/

docker build -t reddit:latest .
```
* Пуш образа в Docker Hub

```
docker tag reddit:latest  <docker hub login>/otus-reddit:1.0
```

* Удаление docker host

```
docker-machine rm docker-host
yc compute instance delete docker-host
```
