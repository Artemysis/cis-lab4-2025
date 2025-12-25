# Лабораторная работа 4

## Установить minikube

### Windows

1. Установить minikube

https://github.com/kubernetes/minikube/releases/latest/download/minikube-installer.exe

### macos

1. Установить homebrew https://brew.sh/
2. Установить minikube `brew install minikube`

## Запустить minikube

```bash
minikube start
```

## Установить расширения

```bash
minikube addons enable ingress
minikube addons enable dashboard
```

## Установить kubectl

### Windows

https://dl.k8s.io/release/v1.35.0/bin/windows/amd64/kubectl.exe

### macos

`brew install kubectl`

## Проверить статус кластера и конфигурацию kubectl

```bash
minikube status
kubectl cluster-info
```

## Создать Namespace

```bash
kubectl create ns lab4
kubectl get ns
```

# Императивно создать Pod

```bash
kubectl run my-pod-1 --image=nginx:alpine --restart=Never -n lab4
kubectl get pods -n lab4
```

## Императивно создать Service

```bash
kubectl expose pod my-pod-1 --port=80 --target-port=80 --name=my-service-1 -n lab4
kubectl get services -n lab4
```

## Императивно создать Ingress

```bash
kubectl create ingress my-ingress-1 -n lab4 \
  --rule="my-1.cluster.local/*=my-service-1:80" \
  --class=nginx

kubectl get ingress -n lab4
```

### Добавить в файл hosts:

```bash
127.0.0.1 my-1.cluster.local
127.0.0.1 my-2.cluster.local
127.0.0.1 my-3.cluster.local
```

## Создать туннель в minikube

```bash
minikube tunnel
```

## Проверить доступность

```
http://my-1.cluster.local
```

## Клонировать репу

1. Сделать форк `https://github.com/ufadev/cis-lab4-2025`
2. Клонировать локально

## Изучить манифесты

Они в каталоге `/k8s`

## Раскатить вторую инсталляцию

```bash
cd k7s
kubectl apply -f .
```

## Проверить доступность

```bash
kubectl get deployment -n lab4
kubectl get ingress -n lab4
kubectl get service -n lab4
kubectl get pod -n lab4
```

```
http://my-1.cluster.local
```

## Попробовать удалить pod

```bash
kubectl delete Pod <pod name> -n lab4
```

## Убедиться что под восстанавливается

```bash
kubectl get events -n lab4
```

## Добавить реплику

1. Увеличить количество `replicas` до `2` в манифесте deployment.
2. Раскатить повторно используя `kubectl apply`
3. Проверить результат

## Установить helm

### Windows
Скачать и распаковать `https://github.com/helm/helm/releases`

### macos
```bash
brew install helm
```

## Раскатить приложение используя helm

```bash
cd helm
helm install my-app . -n lab4
```

Проверить доступность

```bash
helm list -n lab4

helm status my-app -n lab4

kubectl get all -n lab4
```

## Увеличить количество реплик

1. В файле `values.yaml` установить количество реплик в `3`
2. Обновить релиз

```bash
helm upgrade my-app ./my-app -n lab4 -f values.yaml
```

3. Проверить состояние

## Отчетность

1. Собрать логи

```bash
kubectl get deployment -n lab4
kubectl get ingress -n lab4
kubectl get service -n lab4
kubectl get pod -n lab4
kubectl get events -n lab4
```

2. Положить их в репозиторий в файл `results.log`

## dashboard

1. Выполнить

```bash
minikube dashboard
```

2. Проанализировать объекты неймспейса lab4

## Удалить релиз

1. Выполнить

```bash
helm uninstall my-app -n lab4
```

2. Убедиться что созданные объекты удалены