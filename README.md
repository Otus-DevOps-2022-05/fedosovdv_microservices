# fedosovdv_microservices
fedosovdv microservices repository
## ДЗ-19: (kubernetes-1)
- созданы манифесты приложений
- созданы vm на YC, установлены kubelet kubeadm kubectl
- на master выполнен init, добавлен worker (после добавления - NotReady)
- в манифаесте calico CALICO_IPV4POOL_CIDR установлено в 10.244.0.0/16 (после установки calico ноды - Ready)
- применены рание созданные манифесты ```kubectl apply -f <filename>```

```
kubectl get pods
NAME                                  READY   STATUS    RESTARTS   AGE
comment-deployment-6989ff6cc5-fbqm9   1/1     Running   0          63s
mongo-deployment-797dcbffd4-kgz4c     1/1     Running   0          57m
post-deployment-b5f4dc6cf-xq42z       1/1     Running   0          71s
ui-deployment-6647cbff5-7vbh2         1/1     Running   0          57s
```

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

## ДЗ-21: (kubernetes-3)

- установлен Ingress контроллер
- добавлены ingress манифесты  
- создан сертификат
- ограничен доступ к mongodb
- создан диск в YC, создан persistent volume для mongo


## ДЗ-20: (kubernetes-2)
- развернуто локальное окружение для работы с Kubernetes на minikube
- развернут Kubernetes в YC  
- деплой reddit в Kubernetes YC (скины в kubernetes/screenshot)


## ДЗ-19: (kubernetes-1)
- созданы манифесты приложений
- созданы vm на YC, установлены kubelet kubeadm kubectl
- на master выполнен init, добавлен worker (после добавления - NotReady)
- в манифаесте calico CALICO_IPV4POOL_CIDR установлено в 10.244.0.0/16 (после установки calico ноды - Ready)
- применены рание созданные манифесты ```kubectl apply -f <filename>```

```
kubectl get pods
NAME                                  READY   STATUS    RESTARTS   AGE
comment-deployment-6989ff6cc5-fbqm9   1/1     Running   0          63s
mongo-deployment-797dcbffd4-kgz4c     1/1     Running   0          57m
post-deployment-b5f4dc6cf-xq42z       1/1     Running   0          71s
ui-deployment-6647cbff5-7vbh2         1/1     Running   0          57s
```


## ДЗ-16 (gitlab-ci-1)
- создана ВМ через cli yandex
- настроен GitLab(получен пароль root; закрыта регистрация)
- добавлен Reddit в проект
- на локальную машину (ввиду нехватки памяти для тестов) поставлен раннер
- добавлена зависимость от тегов для production и stage
- добавлены динамические окружения



## ДЗ-20: (kubernetes-2)
- развернуто локальное окружение для работы с Kubernetes на minikube
- развернут Kubernetes в YC  
- деплой reddit в Kubernetes YC (скины в kubernetes/screenshot)


## ДЗ-19: (kubernetes-1)
- созданы манифесты приложений
- созданы vm на YC, установлены kubelet kubeadm kubectl
- на master выполнен init, добавлен worker (после добавления - NotReady)
- в манифаесте calico CALICO_IPV4POOL_CIDR установлено в 10.244.0.0/16 (после установки calico ноды - Ready)
- применены рание созданные манифесты ```kubectl apply -f <filename>```

```
kubectl get pods
NAME                                  READY   STATUS    RESTARTS   AGE
comment-deployment-6989ff6cc5-fbqm9   1/1     Running   0          63s
mongo-deployment-797dcbffd4-kgz4c     1/1     Running   0          57m
post-deployment-b5f4dc6cf-xq42z       1/1     Running   0          71s
ui-deployment-6647cbff5-7vbh2         1/1     Running   0          57s
```

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
