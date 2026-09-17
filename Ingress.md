## Ingress Management
### Get All Ingress
```
$ kubectl get ingress --all-namespaces
or
$ kubectl get ingress -A
```
### Get Ingress by name
```
$ kubectl get ingress hello-world
```
### Get Ingress in yaml format
```
$ kubectl get ingress hello-world -o yaml
```

### Create Ingress
```
$ kubectl apply -f -<<'EOF
apiVersion: networking.k8s.io/v1
metadat:
  name: hello
  namespace: default
spec:
  ingressClassName: nginx
  rules:
    - host: hello.local
      http:
        paths:
          - path:/hello
            pathType: Prefix
            backend:
              service:
                name: hello-world
                port:
                  number: 8080
EOF
```
### Enable Minikube addon
> Why Needed: The ingress manifest only creates routing rules. It does not create the component that receives HTTP trafic. It installs the NGINX Ingress Controller- the actual reverse proxy that reads and enforce your ingress rules
```
$ minikube addons enable ingress
```

### Check Minikube addons
#### List all addons
```
$ minikube addons list
```
#### List particular addons
```
$ minikube addons list | grep ingress
```

### Make hosts entry for local dns
> Adds a local DNS override because hello.local is not public DNS. Without it, your shell/browser does not know which IP address hello.local should use.
> Note: Order does not matter much- you can apply the manifest before or after enabling addon. The rule will begin working only after both the controller is running and hello.local resolves to the minikube IP.
```
$ echo "$(minikube ip) hello.local" | sudo tee -a /etc/hosts
```
### Edit Ingress in yaml format
```
$ kubectl edit ingress hello-world 
```
