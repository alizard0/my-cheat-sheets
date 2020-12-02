# openShift operators-sdk

1. Install operator-sdk 
```
sudo wget https://github.com/operator-framework/operator-sdk/releases/download/v0.16.0/operator-sdk-v0.16.0-x86_64-linux-gnu -O /usr/local/bin/operator-sdk`
sudo chmod +x /usr/local/bin/operator-sdk
```
1. Create operator (for getting structure files)
```
operator-sdk new app-operator --api-version=app.example.com/v1alpha1 --kind=App
```
2. Deploy operator
```
// brand new project
oc new-project ${GUID}-operator --display-name="Operator"
// deploy the service account used by our operator
oc apply -f $HOME/operator/deploy/service_account.yaml
// deploy the role and role-binding for grant the correct permissions to the operator
// note: you might need to reduce the scope here by applying the role just for the resources types you want to (Kind)
// if so, edit role.yaml and under apiGroups.resources, set the resource_types (known as Kind)
oc apply -f $HOME/operator/deploy/role.yaml
oc apply -f $HOME/operator/deploy/role_binding.yaml
// deploy the operator
// note: you need to update the image path
oc apply -f $HOME/operator/deploy/operator.yaml
```
3. Use operator through a Custom Resource file
```
// Create Custom Resource (save this to customResource.yaml)
apiVersion: <your-api-version>
kind: <your-kind>
metadata:
  name: <your-name>
spec:
  <specs-list>
// Make request by applying this file
oc apply -f $HOME/operator/customResource.yaml
```

