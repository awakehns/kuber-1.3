# Домашнее задание к занятию `«Запуск приложений в K8S»` - `Демин Герман`

## Задание 1

`minikube start`

`nano deployment.yamll`

[deployment.yaml](deployment.yaml)

`kubectl apply -f deployment.yaml`

`kubectl get pods`

`kubectl get deploy`

Контейнер multitool падает после запуска. Смотрим логи

![kube-logs-1](/img/kube-logs-1.png)

Причина ясна: оба контейнера в одном поде ссылаются на 80 порт. Изучив контейнер я понял, что multitool внутри себя тоже запускает nginx, а сеть у них одна, поэтому происходит конфликт портов. Смысла держать два nginx, жестко привязанных к 80 порту нет, да и вообще нет в большинстве случаев, так что принимаю решение убрать отдельный nginx, оставив комплексный multitool.

`kubectl delete deployment nginx-multitool`

`kubectl apply -f deployment.yaml`

`kubectl get pods`

`kubectl scale deployment nginx-multitool --replicas=2`

`kubectl get pods`

![kube-1](/img/kube-1.png)

Делаем сервис

`nano netology-service.yaml`

[netology-service.yaml](netology-service.yaml)

`kubectl apply -f netology-service.yaml`

`kubectl get services`

`nano multitool-test.yaml`

[multitool-test.yaml](multitool-test.yaml)

`kubectl apply -f multitool-test.yaml`

`kubectl exec -it multitool-test -- curl http://multitool-svc`

![kube-2](/img/kube-2.png)



## Задание 2

`nano nginx-init-deployment.yaml`

[nginx-init-deployment.yam](nginx-init-deployment.yam)

`nano nginx-init-service.yaml`

[nginx-init-service.yaml](nginx-init-service.yaml)

`kubectl apply -f nginx-init-deployment.yaml`

`kubectl get pods`

`kubectl apply -f nginx-init-service.yaml`

`kubectl get pods`

![kube-3](/img/kube-3.png)
