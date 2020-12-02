# openshift ci-cd

1. Login into your Registry
```
expose REGISTRY=<your-registry-path>
podman login -u <username> -p <pwd> ${REGISTRY}
```

2. Deploy Nexus, a repository artifact manager
```
oc new-app sonatype/nexus
oc expose svc/nexus
oc get routes
```
