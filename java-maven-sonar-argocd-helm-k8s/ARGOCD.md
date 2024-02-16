[For the ARGO CD](https://www.youtube.com/watch?v=ZgJQG475oME&t=981s)
Watch this tutorial for full reference

Install the ARGO CD in different namespace\ 
it is always suggested, whenever one needs to install the kubernetes controller, create the different namespace\
to prevent from the mess from different namespaces
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
\
To get all the pods for the namespace we created
```
kubectl get pods -n argocd
```
shows output like this
```
NAME                                                READY   STATUS              RESTARTS   AGE
argocd-application-controller-0                     0/1     ContainerCreating   0          9s
argocd-applicationset-controller-64c77cff5d-rc9sc   0/1     ContainerCreating   0          10s
argocd-dex-server-5fb9857f6c-mflcp                  0/1     Init:0/1            0          10s
argocd-notifications-controller-679d59bf4b-w5mwm    0/1     ContainerCreating   0          10s
argocd-redis-66d9777b78-vp4pn                       0/1     ContainerCreating   0          10s
argocd-repo-server-554bd7bf5-cxwwb                  0/1     Init:0/1            0          10s
argocd-server-6697c5749b-xkklc                      0/1     ContainerCreating   0          9s
```
\
To get the services created
```
kubectl get svc -n argocd
```
Output
```
NAME                                      TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
argocd-applicationset-controller          ClusterIP   10.104.74.217    <none>        7000/TCP,8080/TCP            2m20s
argocd-dex-server                         ClusterIP   10.109.36.54     <none>        5556/TCP,5557/TCP,5558/TCP   2m20s
argocd-metrics                            ClusterIP   10.111.181.87    <none>        8082/TCP                     2m20s
argocd-notifications-controller-metrics   ClusterIP   10.108.127.8     <none>        9001/TCP                     2m20s
argocd-redis                              ClusterIP   10.101.24.26     <none>        6379/TCP                     2m20s
argocd-repo-server                        ClusterIP   10.104.255.178   <none>        8081/TCP,8084/TCP            2m20s
argocd-server                             ClusterIP   10.110.45.137    <none>        80/TCP,443/TCP               2m20s
argocd-server-metrics                     ClusterIP   10.109.65.252    <none>        8083/TCP                     2m20s
```
\
Edit the service "argocd-server" to change the "Type" from "ClusterIP" mode to "NodePort" mode, Run the command
```
kubectl edit svc argocd-server -n argocd
```
It will open the file for editing, change the type from "ClusterIP" to "NodePort", then save it\
\
Now, need to expose the server, need to do the portforwarding or the tunneling
```
minikube service list -n argocd
```
\
Then, run
```
minikube service argocd-server -n argocd
```
Output will be
```
W0216 13:38:10.575817   20452 main.go:291] Unable to resolve the current Docker CLI context "default": context "default": context not found: open C:\Users\shikh\.docker\contexts\meta\37a8eec1ce19687d132fe29051dca629d164e2c4958ba141d5f4133a33f0688f\meta.json: The system cannot find the path specified.
|-----------|---------------|-------------|---------------------------|
| NAMESPACE |     NAME      | TARGET PORT |            URL            |
|-----------|---------------|-------------|---------------------------|
| argocd    | argocd-server | http/80     | http://192.168.49.2:31501 |
|           |               | https/443   | http://192.168.49.2:30586 |
|-----------|---------------|-------------|---------------------------|
* Starting tunnel for service argocd-server.
|-----------|---------------|-------------|------------------------|
| NAMESPACE |     NAME      | TARGET PORT |          URL           |
|-----------|---------------|-------------|------------------------|
| argocd    | argocd-server |             | http://127.0.0.1:14249 |
|           |               |             | http://127.0.0.1:14250 |
|-----------|---------------|-------------|------------------------|
[argocd argocd-server  http://127.0.0.1:14249
http://127.0.0.1:14250]
! Because you are using a Docker driver on windows, the terminal needs to be open to run it.
```
Copy the argocd-server url that starts with "http://127.0.0.1" to the browser\
\
To login directly into the ARGO CD without any Identity Provider
Open another Terminal window or tab\
To get the ARGO CD UI Password 
```
kubectl get secret -n argocd
```
Output will be
```
NAME                          TYPE     DATA   AGE
argocd-initial-admin-secret   Opaque   1      15m
argocd-notifications-secret   Opaque   0      16m
argocd-secret                 Opaque   5      16m
```
\
Now to get the password which is present in "argocd-initial-admin-secret" file, we need to open it\
Then for this, run the command
```
kubectl edit secret argocd-initial-admin-secret  -n argocd
```
It will open the file, and copy the password which is next to "password:"
\
Decode it with base 64, using the command
```
echo <your_password> | base64 --decode
```
It will output your password, copy it and put it into your browser.
