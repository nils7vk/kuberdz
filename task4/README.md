[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://github.com/nils7vk/kuberdz/tree/master/task4)

## Create deploy_view_user
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
## Create deploy_edit_user
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

## Create prod_admin

```sh 
openssl genrsa -out prod_admin.key 2048 
```
```sh
openssl req -new -key prod_admin.key -out prod_admin.csr -subj "/CN=prod_admin"
```
```sh
openssl x509 -req -in prod_admin.csr -CA ~/.minikube/ca.crt -CAkey ~/.minikube/ca.key -CAcreateserial -out prod_admin.crt -days 500
```
```sh
kubectl config set-credentials prod_admin --client-certificate=prod_admin.crt --client-key=prod_admin.key
```
```sh
kubectl config set-context prod_admin --cluster=minikube --user=prod_admin
```
```sh
vim ~/.kube/config
```
```sh
- name: prod_admin
  user:
    client-certificate: /Users/vitalyk/kuber_homework/task4/prod_admin.crt
    client-key: /Users/vitalyk/kuber_homework/task4/prod_admin.key
contexts:
- context:
    cluster: minikube
    user: prod_admin
  name: prod_admin
```

## Create prod_view
```sh
openssl genrsa -out prod_view.key 2048
```
```sh
openssl req -new -key prod_view.key -out prod_view.csr -subj "/CN=prod_view"
```
```sh
openssl x509 -req -in prod_view.csr -CA ~/.minikube/ca.crt -CAkey ~/.minikube/ca.key -CAcreateserial -out prod_view.crt -days 500
```
```sh
kubectl config set-credentials prod_view --client-certificate=prod_view.crt --client-key=prod_view.key
```
```sh
kubectl config set-context prod_view --cluster=minikube --user=prod_view
```
```sh
vim ~/.kube/config
```
```sh
- name: prod_view
  user:
    client-certificate: /Users/vitalyk/kuber_homework/task4/prod_view.crt
    client-key: /Users/vitalyk/kuber_homework/task4/prod_view.key
contexts:
- context:
    cluster: minikube
    user: prod_view
  name: prod_view
```

## Create serviceAccount sa-namespace-admin

Чтобы получить Service Account Token:
- Создайте ServiceAccount:
```sh
kubectl -n kube-system create serviceaccount sa-namespace-admin
```
- Создайте ClusterRoleBinding и добавьте роль с правами администратора (cluster-admin):
```sh
kubectl create clusterrolebinding sa-namespace-admin --clusterrole=cluster-admin --serviceaccount=kube-system:sa-namespace-admin
```
- Получите имя секрета созданного ServiceAccount, в котором хранится токен:
```sh
export TOKENNAME=$(kubectl -n kube-system get serviceaccount/sa-namespace-admin -o jsonpath='{.secrets[0].name}')
```
- Получите токен из секрета в base64, декодируйте и добавьте в переменную окружения TOKEN:
```sh
export TOKEN=$(kubectl -n kube-system get secret $TOKENNAME -o jsonpath='{.data.token}' | base64 --decode)
```
- Проверьте работоспособность токена, сделайте запрос в Kubernetes API с токеном в заголовке "Authorization: Bearer <TOKEN-HERE>":
```sh
curl -k -H "Authorization: Bearer $TOKEN" -X GET "https://<KUBE-API-IP>:8443/api/v1/nodes" | json_pp
```
- Добавьте сервис-аккаунт в kubeconfig:
```sh
kubectl config set-credentials sa-namespace-admin --token=$TOKEN
```
```sh
kubectl config set-context sa-namespace-admin --cluster=minikube --user=sa-namespace-admin
```
