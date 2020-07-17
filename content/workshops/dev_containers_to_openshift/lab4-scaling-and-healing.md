---
title: Lab 4 - Scaling and Self Healing
workshops: dev_containers_to_openshift
workshop_weight: 41
layout: lab
---

# Scaling and Self-Healing Lab
This lab will focus on the ability to rapidly scale workloads in OpenShift, as well as demonstrating OpenShift's ability to very quickly heal from pods which have failed.  Unlike prior labs, this lab will have both command line and web UI components, so you can experience these capabilities from either environment.

# Set up the lab environment
We need to set up the environment for this lab

## Login via the "oc" command on the command line, if you are not already
> <i class="fa fa-terminal"></i> Use your existing Butterfly terminal, and login, using the lab URI:

```bash
$ oc login {{< urishortfqdn "https://" "api" ":6443" >}} --insecure-skip-tls-verify=true
Authentication required for {{< urishortfqdn "https://" "api" ":6443" >}} (openshift)
 Username: user{{< span "userid" "YOUR#" >}} Password:
Login successful.
```

> <i class="fa fa-terminal"></i> Run the initialization script for the lab:

```bash
$ ./create-lab4-environment.sh
```

## Make sure the environment is correct:

> From the command line, display the current status.

> <i class="fa fa-terminal"></i> Type the following command to show services, deployment configs, build configurations, and active deployments (this will come in handy later):

```bash
$ oc status
```

# Now let's scale out the application by increasing the replica count
> You can easily scale out an application by making replicas of an existing pod.  First, check to see the pods running in the lab environment:

```bash
oc get pods
```
> To scale out the existing pod, use the command:

```bash
oc scale --replicas=2 deployment/lab4-sample-app
```

> Then check the results with the command:

```bash
oc get pods
```

> You should see two running pods.  Now try deleting one of the pods and see what happens:

```bash
oc delete pod/lab4-sample-app
oc get pods -w
```

> Note that, almost immediately, the a new pod is created to replace the deleted pod.  It happens so quickly that it may be difficult for you to see it from the command line.  So reset the environment to a single running pod and try it again from the web UI so you can see it better.

> Reset the replica count on the running pod:
```bash
oc scale --replicas=1 deployment/lab4-sample-app
```

> Check that you are down to a single running pod before continuing:

```bash
oc get pods -w
```

> Now login to the web UI:

> Click "Workloads", then "Deployments", and then "lab4-sample-app" 

> In the deployment overview, click the up arrow twice, resulting in a total of 3 running pods:

> Click one of the running pods:

> Click the "Actions" button in the top right and then select "Delete Pod":

> Now click the "Delete" button in the popup to confirm the pod deletion:

> Quickly switch back to the deployment overview.  If you're fast enough you'll see the pod you deleted unfill a portion of the deployment circle, and then a new pod fill it back up.

> Do the steps to delete a pod again, but this time, try to delete two pods quickly.  Then switch back to the deployment overview again.  Even if you are very quick, you will see that OpenShift will successfully restore the pod count in a couple blinks of an eye!

> You can browse the pods list again to see the old pod was deleted and a new pod has appeared:

> Scale back down to 1 replica. Click the down arrow twice from the Deployments Overview page.

{{< importPartial "footer/footer.html" >}}
