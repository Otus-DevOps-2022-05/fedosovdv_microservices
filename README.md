# fedosovdv_microservices
fedosovdv microservices repository


## ДЗ-13 (docker-3)
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
