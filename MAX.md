How to run MAX on Power
1. Create a new project using the OpenShift GUI. And create the templates.
```
oc new-project max
oc create -f max-image.yml
oc create -f max-image-web-app.yml
```
2. Deploy the Image Caption tile. Wait for it to deploy
3. Create an entry corresponding to the route in your /etc/hosts file
4. Click on the route to ensure that it is deployed
5. Capture the service ip address (port should be 5000)
```
oc get svc
```
6. Deploy the Web App tile. Post the IP address and port on the URL field:
```
http://172.x.x.x:5000
```
7. Wait for the deployment to complete. Create an entry for this route as well in /etc/hosts
8. Click on the route.
