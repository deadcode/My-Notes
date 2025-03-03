# List of Kubernetes CLI commands

## Minikube
* Install
    ```console
    brew install minikube
    ```
* Start minikube:
    ```console
    minikube start
    ```
* Dashboard:
    ```console
    minikube dashboard
    ```
* Login / SSH to cluster:
    ```console
    minikube ssh
    ```
* Pause / Unpause instance
    ```console
    minikube pause
    minikube unpause
    ```
* Stop cluster:
    ```console
    minikube stop
    ```
* Delete (all) clusters:
    ```console
    minikube delete --all
    ```
* Access the service
    ```console
    minikube service <name>
    eg. minikube service hello-minikube
    ```

## Kubectl
* Version:
    ```console
    kubectl version --client
    ```
* Show / get:
    ```console
    kubectl get nodes
    kubectl get pods
    kubectl get pods -o wide
    kubectl get deployments
    kubectl get services
    kubectl get replicasets
    ```
* Show / describe details:
    ```console
    kubectl describe node <name>
    kubectl describe nodes
    kubectl describe pod <name>
    kubectl describe pods
    kubectl describe service <name>
    kubectl describe services
    kubectl describe replicasets
    ```
* Deploy application
    ```console
    kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0
    kubectl expose deployment hello-minikube --type=NodePort --port=8080
    ```
    Apply from YAML file
    ```console
    kubectl apply -f <file-name>
    ```
* Create pod
    ```console
    kubectl run <name> --image=<url/name>
    ```
* Delete 
    ```console
    kubectl delete pod <name>
    kubectl delete replicaset <name>
    ```
* Forward the port (to access the sevice)
    ```console
    kubectl port-forward service/hello-minikube 7080:8080
    ```
* Scale a replicaset
    ```console
    kubectl scale --replicas=<n> -f <replicaset-yaml-file>
    kubectl scale --replicas=<n> replicaset <replica-set-name>
    kubectl edit replicasets
    kubectl edit replicaset <name>
    ```
* Rollout Deployment
    ```console
    kubectl rollout status deployment/<deployment-name>
    kubectl rollout history all
    kubectl rollout undo deployment/<deployment-name>
    ```
* Edit
    ```console
    kubectl edit <deployment-name>
    kubectl set image <deployment-name> <pod-name>=<pod-image-name> e.g. kubectl set image myapp-deployment nginx=nginx:1.9.1
    ```