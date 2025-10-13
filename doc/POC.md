Вводні


```Пілотна версія «AsciiArtify» вже в розробці.

Команда погодилась з вашими аргументами та запросили підготувати Proof of Concept (PoC) по розгортанню GitOps-системи на рекомендованому вами варіанті Kubernetes. Командою запроповано продукт ArgoCD.

PoC — це етап, коли розробники перевіряють, чи є технічна можливість реалізувати концепцію продукту. На цьому етапі розробники використовують мінімальний набір функцій, щоб продемонструвати, що продукт може працювати та виконувати свої основні функції.

Вам потрібно практично розгорнути Kubernetes кластер за допомогою інструменту, що затверджений на етапі Concept. Встановити систему та налаштувати доступ команди до графічного інтерфейсу ArgoCD.

Основні кроки для підготовки та розгортанню ArgoCD на Kubernetes можна знайти у Coding Session.

Результатом завдання буде встановлена та налаштована система ArgoCD, готова до реалізації MVP.

Відповіддю на завдання буде посилання на репозиторій AsciiArtify (формат посилання: https://github.com/<username>/AsciiArtify) з демо-інструкцією на отримання доступу до інтерфейсу ArgoCD. Файл doc/POC.md у форматі Markdown, гілка main (Приклад демо з офіційного сайту — https://argo-cd.readthedocs.io/en/stable/)
```

'''$ k3d cluster create argo
... 
INFO[0029] Cluster 'argo' created successfully!         
INFO[0029] You can now use it like this: kubectl cluster-info

$ kubectl cluster-info
Kubernetes control plane is running at https://0.0.0.0:43297
CoreDNS is running at https://0.0.0.0:43297/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
Metrics-server is running at https://0.0.0.0:43297/api/v1/namespaces/kube-system/services/https:metrics-server:https/proxy

$ k version
$ k get all -A


$ kubectl create namespace argocd
namespace/argocd created

$ k get ns
NAME              STATUS   AGE
kube-system       Active   80m
default           Active   80m
kube-public       Active   80m
kube-node-lease   Active   80m
argocd            Active   28s

$ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

$ k get all -n argocd

# перевіримо статус контейнерів: 
$ k get pod -n argocd -w
NAME                                                READY   STATUS    RESTARTS   AGE
argocd-redis-b5d6bf5f5-sqlgw                        1/1     Running   0          3m41s
argocd-notifications-controller-589b479947-fm4kp    1/1     Running   0          3m41s
argocd-applicationset-controller-6f9d6cfd58-9rvjf   1/1     Running   0          3m41s
argocd-dex-server-6df5d4f8c4-nd829                  1/1     Running   0          3m41s
argocd-application-controller-0                     1/1     Running   0          3m40s
argocd-server-7459448d56-xnrvf                      1/1     Running   0          3m40s
argocd-repo-server-7b75b45897-qsw9z                 1/1     Running   0          3m41s


$ kubectl port-forward svc/argocd-server -n argocd 8080:443&
[1] 23820
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
Handling connection for 8080
'''

$ k -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"

[![asciicast](https://asciinema.org/a/laA84I5umhQOWrQJYxwFgsM0G.svg)](https://asciinema.org/a/laA84I5umhQOWrQJYxwFgsM0G)
cFZIWTM3eFpJR3hDeUxLNQ==#                                                                                                        
$ k -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"|base64 -d;echo
pVHY37xZIGxCyLK5
