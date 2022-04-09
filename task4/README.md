

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)



## deploy_view_user
```sh
openssl genrsa -out deploy_view_user.key 2048
```
```sh
openssl req -new -key deploy_view_user.key -out deploy_view_user.csr -subj "/CN=deploy_view_user"
```
```sh
openssl x509 -req -in deploy_view_user.csr -CA ~/.minikube/ca.crt -CAkey ~/.minikube/ca.key -CAcreateserial -out deploy_view_user.crt -days 500
```
```sh
kubectl config set-credentials deploy_view_user --client-certificate=deploy_view_user.crt --client-key=deploy_view_user.key
```
```sh
kubectl config set-context deploy_view_user --cluster=minikube --user=deploy_view_user
```
```sh
kubectl create clusterrole deploy_view_user --verb=get,list,watch --resource=pods,deployments
```
```sh
vim ~/.kube/config
```
```sh
- name: deploy_view
  user:
    client-certificate: /Users/vitalyk/kuber_homework/task4/deploy_view_user.crt
    client-key: /Users/vitalyk/kuber_homework/task4/deploy_view_user.key
contexts:
- context:
    cluster: minikube
    user: deploy_view_user
  name: deploy_view_user
```
## deploy_edit_user
```sh
openssl genrsa -out deploy_edit_user.key 2048
```
```sh
openssl req -new -key deploy_edit_user.key -out deploy_edit_user.csr -subj "/CN=deploy_edit_user"
```
```sh
openssl x509 -req -in deploy_edit_user.csr -CA ~/.minikube/ca.crt -CAkey ~/.minikube/ca.key -CAcreateserial -out deploy_edit_user.crt -days 500
```
```sh
kubectl config set-credentials deploy_edit_user --client-certificate=deploy_edit_user.crt --client-key=deploy_edit_user.key
```
```sh
kubectl config set-context deploy_edit_user --cluster=minikube --user=deploy_edit_user
```
```sh
kubectl create clusterrole deploy_edit_user --verb=get,list,watch,create,update,patch,delete,deletecollection --resource=pods,deployments
```
```sh
vim ~/.kube/config
```
```sh
- name: deploy_edit_user
  user:
    client-certificate: /Users/vitalyk/kuber_homework/task4/deploy_edit_user.crt
    client-key: /Users/vitalyk/kuber_homework/task4/deploy_edit_user.key
contexts:
- context:
    cluster: minikube
    user: deploy_edit_user
  name: deploy_edit_user
```

## Tech
