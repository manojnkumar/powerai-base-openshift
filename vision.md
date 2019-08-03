How to run PowerAI Vision demo on Power
1. Follow Steps 1 through 5 in https://github.com/manojnkumar/powerai-base-openshift/blob/master/README.md
2. Create the template in the openshift project

```
oc project openshift
oc create -f max.yml
```

3. Create a new project using the OpenShift GUI. And create the templates.
```
oc new-project vision
oc create -f max-live.yml
```
4. Deploy the PowerAI Vision tile. Wait for it to deploy
5. Create an entry corresponding to the route in your /etc/hosts file
6. Click on the route to ensure that it is deployed
