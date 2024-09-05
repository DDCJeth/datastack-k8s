## [airbyte](https://docs.airbyte.com/)


### [How to deploy](https://docs.airbyte.com/deploying-airbyte/on-kubernetes-via-helm)


#### 1) Add Chart repo

```
helm repo add airbyte https://airbytehq.github.io/helm-charts
helm repo update
helm search repo airbyte

# To see all available versions
helm search repo airbyte --versions
```





#### 2) Create namespace and switch in

```
k create ns airbyte
k config set-context --current --namespace=airbyte
```


#### 3) Create a values override file

- Get default values
```
AIRBYTE_VERSION="0.524.0"
helm show values airbyte/airbyte --version $AIRBYTE_VERSION > values-airbyte.yaml
```

- Create new values file and edit if necessary

#### 4) Install airbyte

```
# For use default values
AIRBYTE_VERSION="0.524.0"
helm install airbyte airbyte/airbyte --version $AIRBYTE_VERSION --namespace airbyte --debug

# For use edited values file named values.yaml
helm install airbyte airbyte/airbyte --version $AIRBYTE_VERSION --namespace airbyte --values ./values.yaml --debug
```


#### 5) Set up port-forward for test

```
export POD_NAME=$(kubectl get pods --namespace airbyte -l "app.kubernetes.io/name=webapp" -o jsonpath="{.items[0].metadata.name}")
export CONTAINER_PORT=$(kubectl get pod --namespace airbyte $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
echo "Visit http://127.0.0.1:8080 to use your application"
kubectl --namespace airbyte port-forward $POD_NAME 8080:$CONTAINER_PORT
```


#### 6) 

```
k get secret airbyte-airbyte-secrets -o jsonpath="{.data.DATABASE_PASSWORD}" | base64 -d
k get secret airbyte-airbyte-secrets -o jsonpath="{.data.DATABASE_USER}" | base64 -d
k get secret airbyte-airbyte-secrets -o jsonpath="{.data.DEFAULT_MINIO_ACCESS_KEY}" | base64 -d
k get secret airbyte-airbyte-secrets -o jsonpath="{.data.DEFAULT_MINIO_SECRET_KEY}" | base64 -d
```






#### Airbyte kubernetes components

minio
postgresql
airbyte/airbyte                         0.524.0         0.64.1          Helm chart to deploy airbyte                      
airbyte/airbyte-api-server              0.293.4         0.63.8          Helm chart to deploy airbyte-api-server           
airbyte/airbyte-bootloader              0.543.1         0.64.2          Helm chart to deploy airbyte-bootloader           
airbyte/airbyte-cron                    0.40.37         0.40.17         Helm chart to deploy airbyte-cron                 
airbyte/airbyte-workload-api-server     0.49.18         0.50.33         Helm chart to deploy airbyte-api-server           
airbyte/connector-builder-server        0.543.1         0.64.2          Helm chart to deploy airbyte-connector-builder-...
airbyte/cron                            0.543.1         0.64.2          Helm chart to deploy airbyte-cron                 
airbyte/keycloak                        0.543.1         0.64.2          Helm chart to deploy airbyte-keycloak             
airbyte/keycloak-setup                  0.543.1         0.64.2          Helm chart to deploy airbyte-keycloak-setup       
airbyte/metrics                         0.543.1         0.64.2          Helm chart to deploy airbyte-metrics              
airbyte/pod-sweeper                     0.543.1         0.64.2          Helm chart to deploy airbyte-pod-sweeper          
airbyte/server                          0.543.1         0.64.2          Helm chart to deploy airbyte-server               
airbyte/temporal                        0.543.1         0.64.2          Helm chart to deploy airbyte-temporal             
airbyte/webapp                          0.543.1         0.64.2          Helm chart to deploy airbyte-webapp               
airbyte/worker                          0.543.1         0.64.2          Helm chart to deploy airbyte-worker               
airbyte/workload-api                    0.50.3          0.50.35         Helm chart to deploy the workload-api service     
airbyte/workload-api-server             0.543.1         0.64.2          Helm chart to deploy the workload-api service     
airbyte/workload-launcher               0.543.1         0.64.2          Helm chart to deploy airbyte-workload-launcher






### Links

https://docs.airbyte.com/understanding-airbyte/high-level-view
