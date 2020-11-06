# OpenShift 4

**Login**

`oc login -u ${OC_USER} -p ${OC_PWD} ${API}`

**Create project**

`oc new-project <project-name>`

**Use project**

`oc project <project-name>`

**Create application pods**

`oc new-app --docker-image=<registry-url> --name=<app-name>`    // from docker registry

`oc new-app <public-git-repo-url> --name=<app-name>`            // from a public git repo hosted for instance on GitHub

**Manage pods**

`oc describe pod <pod-name>`                                    // fetch details for a pod

`oc get pods`                                                   // fetch available pods

`oc delete pod <pod-name>`                                      // delete pod

`oc logs -f <pod-name>`                                         // get pod logs

**Get services**

`oc get svc`                                                    // list project services

`oc describe service <service-name>`                            // describe service

**Basic networking**

`oc expose service <service-name>`                               // exposes a public route for service

`oc get routes`

`oc port-forward <app-name> <from>:<to>`                          // forwards requests

**Quotas**

`oc get appliedclusterresourcequotas`                            // list available quotas

`oc describe appliedclusterresourcequotas <quota-id>`            // describe quota

**Documentation**

`oc explain <object-kind>` // get docs

**Deployments**

`oc create -f <deploymnet.yaml>` // run your deployment yaml file

`oc set image deployment <deployment-name> CONTAINER=<docker-image-path>` // updates docker image for a deployment

`oc scale deployment <deployment-name> --replicas=<number-of-replicas>` // scale up by adding more replicas
