How to run PowerAI on OpenShift

1) Clone this repository

```
git clone https://github.ibm.com/redstack-power/demo-powerai-base
```

2) Install CUDA and other software as described in PowerAI docs.

https://hub.docker.com/r/ibmcom/powerai/

3) Enable device-plugin as documented in:

https://blog.openshift.com/how-to-use-gpus-with-deviceplugin-in-openshift-3-10/

4) ON RHEL 7.6:

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

5) Create an imagestream in the 'openshift' project, using this file.
```
$ cd demo-powerai-base
$ oc project openshift
$ oc create -f powerai-base.yml
```

6) Create a project, add a service account in the project (mysvcacct)
```
$  oc new-project test3
$  oc create serviceaccount mysvcacct -n test3
```

7) Give it privileges:
```
$ oc adm policy add-scc-to-user anyuid system:serviceaccount:test3:mysvcacct
$ oc adm policy add-scc-to-user privileged system:serviceaccount:test3:mysvcacct
```

8) Edit the deployment config file, and reference the service account. Use the appropriate templatee for the framework you are interested.

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
