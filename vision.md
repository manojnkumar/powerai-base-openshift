How to run PowerAI Vision demo on Power
1. Create the template in the openshift project

```
oc project openshift
oc create -f max.yml
```

2. Create a new project using the OpenShift GUI. And create the templates.
```
oc new-project vision
oc create -f max-live.yml
```
3. Deploy the PowerAI Vision tile. Wait for it to deploy
4. Create an entry corresponding to the route in your /etc/hosts file
5. Click on the route to ensure that it is deployed
