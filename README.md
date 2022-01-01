# kubernetes_workshop

## build the local images and push them to dockerhub to use them later by kubernetes

### start the minikube
```
minikube start
```

### build the nodejs app 

* get in the root project and run the command below:
```
docker build -t nodeapp .
```
* tag the image 

```
docker tag nodeapp your_dockerhub_user/nodeapp 
```
* login to your dockerhub account

```
docker login -u your_username -p
```
* then enter your password

* push the image to the dockerhub
```
docker push your_dockerhub_user/nodeapp 
```
* repeat the same procedure on the web folder in the project to build custom nginx app (change the name of the image)


### create deployments and services

* make sure that the kubectl works properly, by typing the command :
```
kubectl version
```

* to list the running pods, use the below command:
```
kubectl get pods
```

* to list deployments, run below command:
```
kubectl get deployments
```
* to list services 
```
kubectl get svc
```

* all above commands are used to list (pods, deployments, services) on the default namespace
* to list the ones that are on other name spaces add the -n name_of_namespace to the previous commands

* create interaction terminal in the container

```
kubectl exec -it name_of_pod bash
```
* if you are running multiple containers in the pod, use the below command instead
```
kubectl exec -it name_of_pod -c name_of_container bash
```

* to create kubernetes workload configured on yaml file, we use the below command:
````
kubectl apply -f name_of_file.yaml 
```
* to delete the previous workload use the below command:
```
kubectl delete -f name_of_file_of_that_workload.yaml
```
## Rolling back deployment

* to set the status of the rollou, we use :
```
kubectl rollout status deployment/name_of_deployment
```
* to see the history of the rollout:
```
kubectl rollout history deployment/name_of_deployment
```
* roll back to previous revision
```
kubectl rollout undo deployment/name_of_deployment
```

* roll out to specific revision:
```
kubectl rollout undo deployment/name_of_deployment --to-revision=x
```

* scaling deployment
```
kubectl scale deployment/name_of_deployment --replicas=10
```
* auto scaling (horizantal scaling )
```
kubectl autoscale deployment/nginx-deployment --min=6 --max=15 --cpu-percent=50
```

### implementation 

* run our deployments and services after we have pushed our images to dockerhub

````
kubectl apply -f kubernetes/deployments-services.yaml
```
* check wether if all the pods running succefully

```
kubectl get pods
```
* to see your app, type below command
```
minikube service --url name_of_your_service_of_nginx_deployment (
