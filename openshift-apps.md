# openshift applications

1. create application from git
```
oc new-app --template=eap73-basic-s2i --param APPLICATION_NAME=<app-name> --param SOURCE_REPOSITORY_URL=<path-to-git-file>--param SOURCE_REPOSITORY_REF=<branch-name> --param CONTEXT_DIR=/ --param MAVEN_MIRROR_URL=<maven-path> --as-deployment-config=true
```
2. create secrets
```
oc create secret generic <your-secret-name> --from-literal=username=<user_name> --from-literal=password=<password>
// associate secret to your app
oc set build-secret --source bc/<build-config-app-name> <your-secret-name>
```
3. incremental build (optional)
```
oc patch bc/<build-config-app-name> --patch='{"spec": {"strategy": {"sourceStrategy": {"incremental": true}}}}'
```
4. prevent pulling everytime it builds (optional)
```
oc patch bc/<build-config-app-name> --patch='{"spec": {"strategy": {"sourceStrategy": {"forcePull": false}}}}'
```
5. create application from image stream
```
oc new-app --image-stream=<your-image-stream> <your-git-file-path> --as-deployment-config=true
oc expose svc <app-name>
```
