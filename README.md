# Exercise
#### 1) We want to deploy two containers that scale independently from one another. Container 1: This container runs code that runs a small API that returns users from a database. Container 2: This container runs code that runs a small API that returns shifts from a database.

##### - I have deployed two containers as an individual deployments- one for the users and other for shifts. The cpu limits for both containers are defined.The deployments are exposed as a services.
<br> 

#### 2) For the best user experience auto scale this service when the average CPU reaches 70%.
##### - To autoscale the services, two HorizontalPodAutoScaler (hpa) are created- one for each deployment. The average utilization for hpa is set to 70%. To test the working of hpa, two client loadgenerator pods are created. The container within the client pod runs in an infinite loop, sending queries to the individual deployment services. 
<br>

#### 3) Ensure the deployment can handle rolling deployments and rollbacks.
##### -To minimize the downtime of the deployment Rolling Update Strategy is used that incrementally update pod instances with new ones. The deployments have been tested through kubectl command line to ensure that they can handle rollouts and rollbacks. 
##### Rollout: `kubectl set image deployment/deployment-1 nginx:1.16.1`

##### Rollout status: `kubectl rollout status deployment/deployment-1`

##### Rolling Back: `kubectl rollout undo deployment/deployment-1`
<br>

#### 4) Your development team should not be able to run certain commands on your k8s cluster, but you want them to be able to deploy and roll back. What types of IAM controls do you put in place?
##### -To allow development team to only be able to deploy and rollback deployment, Role Based Access Control (RBAC) (Authorization IAM) is used. A clusterole and clusterolebinding (user- devteam) are created that allows development team to only create and update deployments within the cluster.
##### Creating Clusterrole: `kubectl create clusterrole deployment-role --verb=create,update --resource=deployment`

##### Creating Clusterolebiniding: `kubectl create clusterrolebinding deployment-rolebinding --clusterrole=deployment-role --user=devteam`

##### To check the Authorization for development team we use following commands: `kubectl auth can-i create deployments --as devteam`
<br>

##### **Applying configs to multiple environments:** 
##### - For Kubernetes deployments, Helm Charts can be a great solution for handling multiple environment configurations. Helm packages are a set of Kubernetes manifests (that include templates) plus a set of values for these templates. Normally a Helm chart contains only a single values file (for the default configuration), but we can create different value files for different environments. Helm also allows to add variables and functions inside the template files thus making it best approach for scalable applications.
<br>

##### **Auto-scaling deployment based on network latency:**
##### There are two different ways we can autoscale the deployment based on network latency:
##### 1. The first approach is to deploy an Ingress controller to route the traffic and set up an autoscaling policy so that the number of Ingress controller pods instantly expands and contracts to match traffic fluctuations.
##### 2. The more advanced approach is to use custom metrics. Starting with Kubernetes 1.7, we can configure a HorizontalPodAutoscaler (hpa) to scale based on a custom metric (that is not built in to Kubernetes or any Kubernetes component). The HPA controller then queries for these custom metrics from the Kubernetes API.
<br>






