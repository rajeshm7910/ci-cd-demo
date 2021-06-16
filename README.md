## CBP Demo


### Talk about loan micro services
### Setup Loan app

```
Kubectl apply -f loans-app.yaml
Kubectl apply -f loans-service-external.yaml
Kubectl apply -f loans-service.yaml
```

### Get Token

```
apigeecli token gen -a service-accounts/amer-cs-hybrid-demo32-org-admin.json
apigeecli token cache -a service-accounts/amer-cs-hybrid-demo32-org-admin.json
```

### Create proxy from openapi spec

```
apigeecli apis create -n loans-api -f loans-spec.yaml --skip-policy
```
### Deploy Proxy Revision

```
apigeecli apis deploy-wait -e test -n loans-api -v 1 -r 1
```
### Create API product

````
apigeecli  products create  -e test -n loans-product -p loans-api --legacy --approval auto
````

### Create Developer apps

```
apigeecli apps create -n loans-app -p loans-product -e rajeshmi@google.com
```
### Testing API

```
curl https://amer-cs-hybrid-demo32-test.hybrid-apigee.net/v2/offer
```

### Cleanup Script

```
apigeecli apps delete -n loans-app -i deae49de-4efe-4242-b558-c5741cd23f98
apigeecli  products delete -n loans-product
apigeecli apis undeploy -n loans-api -e test -v 1
apigeecli apis delete -n loans-api -v 1

Kubectl delete -f loans-service-external.yaml
Kubectl delete -f loans-service.yaml
Kubectl delete -f loans-app.yaml
```
