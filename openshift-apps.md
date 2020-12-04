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
5. create application from image stream (java image stream)
```
oc new-app --image-stream=redhat-openjdk18-openshift:1.2 <your-git-file-path> --as-deployment-config=true
oc expose svc <app-name>
```
6. create application from binary build (java image stream)
```
// build project locally and it should produce a jar file for java projects
oc new-build --binary=true --name=<your-app-name> --image-stream=redhat-openjdk18-openshift:1.2
// build project on openshift
oc start-build ola-binary --from-file=<your-jar-path> --follow
// deploy app from the image recently built
oc new-app <app-name> --as-deployment-config=true
// expose (optional)
oc expose svc/<app-name> --port=8080
```
