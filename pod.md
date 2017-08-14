# Pod 

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.7.9
    ports:
    - containerPort: 80
```

Create a pod containing an nginx server [pod-nginx.yaml](yaml/pod-nginx.yaml)

```sh
$ kubectl create -f yaml/pod-nginx.yaml
```

List all pods:
```sh
$ kubectl get pods
$ kubectl get pods --all-namespaces
```
<a name="nginx-service"></a>

Create pod which can be access from outside with the help of service  [pod-service-nginx.yaml](yaml/pod-service-nginx.yaml)

```sh
$ kubectl create -f yaml/pod-service-nginx.yaml
```

to access node first grep the node ip
```
kubectl get nodes -o yaml | grep IP -C 1

# get the nodeport in for which service is linked 
$ kubectl get svc nginx-service -o yaml | grep nodePort -C 5

# output will be like this
spec:
  clusterIP: 10.0.0.28
  ports:
  - name: http
    nodePort: 32072 
    port: 8080
    protocol: TCP
    targetPort: 80
  - name: https
--
--
    port: 8080
    protocol: TCP
    targetPort: 80
  - name: https
    nodePort: 30144
    port: 443
    protocol: TCP
    targetPort: 443
  selector:
```
32072 is node port where our app is running 

so we can access it by 
```
http://<node-ip>:32072    
# http://192.168.99.100:32072/  ip in my case