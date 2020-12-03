# openshift ci-cd

1. Login into your Registry
```
expose REGISTRY=<your-registry-path>
podman login -u <username> -p <pwd> ${REGISTRY}
```

2. Deploy Nexus, a repository artifact manager
```
oc new-app sonatype/nexus3:3.21.2 --name=nexus --as-deployment-config=true --as-deployment-config=true
// create a new volume for your nexus
oc set volume dc/nexus --add --overwrite --name=nexus-volume-1 --mount-path=/nexus-data/ --type persistentVolumeClaim --claim-name=nexus-pvc --claim-size=10Gi
oc expose svc/nexus
oc get routes
// get your nexus admin password
export NEXUS_PASSWORD=$(oc rsh <your-podname> cat /nexus-data/admin.password)
```

3. (optonal) If you want to patch the deployment config of nexus use:
```
oc patch dc nexus --patch=<json>
// for instance to force it recreate instead of rolling
oc patch dc nexus --patch='{ "spec": { "strategy": { "type": "Recreate" }}}'
```

4. Deploy Sonarqube
```
// note: you can deploy the postgres db and link it with sonarq
oc new-app --docker-image=<sonaqube-image-path> --env=SONARQUBE_JDBC_USERNAME=sonar --env=SONARQUBE_JDBC_PASSWORD=sonar --env=SONARQUBE_JDBC_URL=<your-jdbc-url> --labels=app=sonarqube --as-deployment-config=true
// create a persistent volume claim for sonar
oc set volume dc/sonarqube --add --overwrite --name=sonarqube-volume-1 --mount-path=/opt/sonarqube/data/ --type persistentVolumeClaim --claim-name=sonarqube-pvc --claim-size=5Gi
// note: sonarqube requires elasticsearch
oc patch dc/sonarqube --type=merge -p '{"spec": {"template": {"metadata": {"labels": {"tuned.openshift.io/elasticsearch": "true"}}}}}'
```

5. Deploy Jenkins
```
oc new-app jenkins-persistent --param ENABLE_OAUTH=true --param MEMORY_LIMIT=2Gi --param VOLUME_CAPACITY=4Gi --param DISABLE_ADMINISTRATIVE_MONITORS=true --as-deployment-config=true
```
