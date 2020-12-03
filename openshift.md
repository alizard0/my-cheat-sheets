# OpenShift 4

1. Login
```
oc login -u ${OC_USER} -p ${OC_PWD} ${API}
```

2. Create project
```
oc new-project <project-name>
```

3. Use project
```
oc project <project-name>
```

4. Create application pods
```
// from docker registry
oc new-app --docker-image=<registry-url> --name=<app-name>
```

```
// from a public git repo hosted for instance on GitHub
oc new-app <public-git-repo-url> --name=<app-name>
```

5. Manage pods
```
// fetch details for a pod
oc describe pod <pod-name>
```
```
// fetch available pods
oc get pods
```

```
// delete pod
oc delete pod <pod-name>
```                                      

```
// get pod logs
oc logs -f <pod-name>
```

6. Get services

```
// list project services
oc get svc
```                                                    

```
// describe service
oc describe service <service-name>
```

7. Basic networking

```
// exposes a public route for service
oc expose service <service-name>
```

```
oc get routes
```

```
// forwards requests
oc port-forward <app-name> <from>:<to>
```

8. Quotas

```
// list available quotas
oc get appliedclusterresourcequotas
```

```
// describe quota
oc describe appliedclusterresourcequotas <quota-id>
```

9. Documentation

```
// get docs

oc explain <object-kind>
```

10. Deployments

```
// run your deployment yaml file
oc create -f <deploymnet.yaml>
``` 

```
// updates docker image for a deployment
oc set image deployment <deployment-name> CONTAINER=<docker-image-path>
``` 

```
// scale up by adding more replicas
oc scale deployment <deployment-name> --replicas=<number-of-replicas>
``` 

```
// patch a deployment config
oc patch dc <dc-name> --patch=<json>
```

```
// increase resources for deployment config
oc set resources dc <dc-name> --limits=memory=2Gi,cpu=2 --requests=memory=1Gi,cpu=500m
``` 

```
// set readiness probe
oc set probe dc/<dc-name> --readiness --failure-threshold 3 --initial-delay-seconds 60 --get-url=http://:8081/
```

```
// set liveness probe
oc set probe dc/nexus --liveness --failure-threshold 3 --initial-delay-seconds 60 -- echo ok
```  

```
// deploy a docker-image
oc new-app --docker-image=<your-docker-image-path-and-version> --env=<your-env-variable> --labels=<your-labels> --as-deployment-config=true
``` 

11. Volumes

```
// Create persistent volume claim 
oc set volume dc/<your-deployment-config-name> --add --overwrite --name=<your-mount-name> --mount-path=<your-mount-path> --type persistentVolumeClaim --claim-name=<your-mount-name> --claim-size=<your-claim-size>
```  

12. Databases

```
// deploy postgres persistent database
oc new-app --template=postgresql-persistent --param POSTGRESQL_USER=<your-admin> --param POSTGRESQL_PASSWORD=<your-pwd> --param POSTGRESQL_DATABASE=<database-name> --param VOLUME_CAPACITY=4Gi --labels=app=<database-name> --as-deployment-config=true
```
