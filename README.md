How to run PowerAI on OpenShift

1) Clone this repository

```
git clone https://github.com/manojnkumar/powerai-base-openshift
```

2) Install CUDA and other software as described in PowerAI docs.

https://hub.docker.com/r/ibmcom/powerai/

3) If you moved fron nvidia container run-time from 1.3 to 1.4 remove the old run-time hook.

```
cd /usr/libexec/oci/hooks.d
mv nvidia /tmp
```


4) Enable device-plugin as documented in:

https://blog.openshift.com/how-to-use-gpus-with-deviceplugin-in-openshift-3-10/

On OpenShift 3.11, you will need to make a few deviations from the blog:

```
oc create -f ./playbooks/roles/nvidia-device-plugin/files/nvidia-device-plugin-scc.yaml
oc create -f ./blog/gpu/device-plugin/nvidia-device-plugin.yml
```

And finally when you notice that the daemonset pods are not running:

```
oc edit daemonset.apps/nvidia-device-plugin-daemonset
```
And change this line:
```
          allowPrivilegeEscalation: false
```        
to
```
          allowPrivilegeEscalation: true
```
AND add this line:
```
          privileged: true
```

5) ON RHEL 7.6:

Comment out more lines from udev rules:

```
# Memory hotadd request
#SUBSYSTEM!="memory", ACTION!="add", GOTO="memory_hotplug_end"
#PROGRAM="/bin/uname -p", RESULT=="s390*", GOTO="memory_hotplug_end"
#ENV{.state}="online"
#PROGRAM="/bin/systemd-detect-virt", RESULT=="none", ENV{.state}="online_movable"
#ATTR{state}=="offline", ATTR{state}="$env{.state}"
#LABEL="memory_hotplug_end"
```

6) Create an imagestream in the 'openshift' project, using this file.
```
$ cd powerai-base-openshift
$ oc project openshift
$ oc create -f powerai-base.yml
```

7) Create a project, add a service account in the project (mysvcacct)
```
$  oc new-project paib
$  oc create serviceaccount mysvcacct -n paib
```

8) Give it privileges:
```
$ oc adm policy add-scc-to-user anyuid system:serviceaccount:paib:mysvcacct
$ oc adm policy add-scc-to-user privileged system:serviceaccount:paib:mysvcacct
```

9) Edit the deployment config file, and reference the service account. Use the appropriate templatee for the framework you are interested.

For tensorflow
```
$ oc create -f tensorflow.yml
```

For pytorch
```
$ oc create -f pytorch.yml
```

For caffe
```
$ oc create -f caffe.yml
```
