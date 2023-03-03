Exercise
1.) We want to deploy two containers that scale independently from one another.
Container 1: This container runs code that runs a small API that returns users from a database.
Container 2: This container runs code that runs a small API that returns shifts from a database.
2.) For the best user experience auto scale this service when the average CPU reaches 70%.
3.) Ensure the deployment can handle rolling deployments and rollbacks.
4.) Your development team should not be able to run certain commands on your k8s cluster, but you want them to be able to deploy and roll back. What types of IAM controls do you put in place?

Approach to Solution:

I have deployed two containers as an individual deployments- one for the users and other for shifts.
The cpu limits for both containers are defined. 
The deployments are exposed as a service so that the deployments can communicate with other resources within and outside the cluster.
To autoscale the services for best user experience I have created two HorizontalPodAutoScaler (hpa)- one for each deployment. The average utilization for hpa is set to 70%. The hpa will increase or decrease the number of replicas to maintain an average CPU utilization of 70%. 
To test the working of hpa, I have created two client loadgenerator pods. The container within the client pod runs in an infinite loop, sending queries to the individual deployment services. 
Additionally, to minimize the downtime of the deployment I am using Rolling Update strategy that incrementally update pod instances with new ones. The deployments have been tested through kubectl command line to ensure that they can handle rollouts and rollbacks. 
To allow development team to only be able to deploy and rollback deployment I have used Role Based Access Control (RBAC) (Authorization IAM). RBAC is a key security component that allows to create fine-grained permission to manage what actions users can perform on resources within the cluster. I have created a clusterole and clusterolebinding (user- devteam) that allows development team to only create and update deployments within the cluster. 

Ques: How would you apply the configs to multiple environments (staging vs production)?
Ans: For Kubernetes deployments, Helm Charts can be a great solution for handling multiple environment configurations. Helm packages are a set of Kubernetes manifests (that include templates) plus a set of values for these templates. Normally a Helm chart contains only a single values file (for the default configuration), but we can create different value files for different environments. Helm also allows to add variables and functions inside the template files thus making it best approach for scalable applications. 

Ques: How would you auto-scale the deployment based on network latency instead of CPU?
Ans: There are two different ways we can autoscale the deployment based on network latency:
1. The first approach is to deploy an Ingress controller to route the traffic and set up an autoscaling policy so that the number of Ingress controller pods instantly expands and contracts to match traffic fluctuations.
2. The more advanced approach is to use custom metrics. Starting with Kubernetes 1.7, we can configure a HorizontalPodAutoscaler (hpa) to scale based on a custom metric (that is not built in to Kubernetes or any Kubernetes component). The HPA controller then queries for these custom metrics from the Kubernetes API. In order to expose metrics beyond CPU and memory to Kubernetes for autoscaling, we will need an "adapter" that serves the custom metrics API.