# openshift applications

1. create application from git
```
oc new-app --template=eap73-basic-s2i --param APPLICATION_NAME=<app-name> --param SOURCE_REPOSITORY_URL=<path-to-git-file>--param SOURCE_REPOSITORY_REF=<branch-name> --param CONTEXT_DIR=/ --param MAVEN_MIRROR_URL=<maven-path> --as-deployment-config=true
```
