# openshift ci-cd

1. Login into your Registry
```
expose REGISTRY=<your-registry-path>
podman login -u <username> -p <pwd> ${REGISTRY}
```

2. Deploy Nexus
