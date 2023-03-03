#### `kubectl apply -f deployment.yaml`
<br>

#### `kubectl apply -f hpa.yaml`
<br>

#### Rollout: `kubectl set image deployment/deployment-1 nginx:1.16.1`
<br>

#### Rollout status: `kubectl rollout status deployment/deployment-1`
<br>

#### Rolling Back: `kubectl rollout undo deployment/deployment-1`
<br>

#### To allow development team to to be able to deploy and roll back we will use Role Based Access Control (RBAC) (Authorization IAM).

#### Creating Clusterrole: `kubectl create clusterrole deployment-role --verb=create,update --resource=deployment`

#### Creating Clusterolebiniding: `kubectl create clusterrolebinding deployment-rolebinding --clusterrole=deployment-role --user=devteam`
<br>

#### To check Authorization for development team we use following commands:

#### `kubectl auth can-i create deployments --as devteam`

#### `kubectl auth can-i watch deployments --as devteam`