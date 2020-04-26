**Install K8S User Interface**  
  
 1. Download dashboard k8s yml `
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml`  
 2. Start proxy, So that UI is accessible from container  
`kubectl proxy ` 
  3. Access the login with following URL
`http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/#!/login`  
  
By default K8S dashboard config doesn't create any dummy (or) default accounts. On side note that default (or) anonymous doesn't have any access to either POD (or) Volumes. Hence for sandbox we may need to create account which have admin access. In production we may need to configure RBAC (or) Kerberos (or) SAML integration for corporate. 
  
**Create K8S admin account from following 3 commands**  
1. `$ kubectl create serviceaccount dashboard -n default`
2. `$ kubectl create clusterrolebinding dashboard-admin -n default  --clusterrole=cluster-admin  --serviceaccount=default:dashboard ` 
3. `kubectl apply -f dashboard-adminuser.yml`
4. `kubectl apply -f admin-role-binding.yml`
5. Retrieve the admin token and use while login in K8S console `$ kubectl get secret $(kubectl get serviceaccount dashboard -o jsonpath="{.secrets[0].name}") -o jsonpath="{.data.token}" | base64 --decode`    
 Following screenshot shows K8S in local after admin login
 ![](/images/K8S-landingPage.JPG) 
  
**Publishing local images to docker hub**
1. Login using `docker login`
2. Push to docker hub `docker push ganeshgowtham/gowthg:latest`
Following shows CLI verbose while pushing and docker hub
![](/images/docker-push-cli.JPG)
![](/images/docker-push-hub.JPG)


**Spring Boot application**  
[Sample SpringBoot app with Docker setup](https://github.com/ganeshgowtham/Jaeger-springboot)