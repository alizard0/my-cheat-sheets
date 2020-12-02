# openshift ci-cd

1. Login into your Registry
```
expose REGISTRY=<your-registry-path>
podman login -u <username> -p <pwd> ${REGISTRY}
```

2. Deploy Nexus, a repository artifact manager
```
oc new-app sonatype/nexus3:3.21.2 --name=nexus --as-deployment-config=true --as-deployment-config=true
oc expose svc/nexus
oc get routes
```

3. (optonal) If you want to patch the deployment config of nexus use:
```
oc patch dc nexus --patch=<json>
// for instance to force it recreate instead of rolling
oc patch dc nexus --patch='{ "spec": { "strategy": { "type": "Recreate" }}}'
```
