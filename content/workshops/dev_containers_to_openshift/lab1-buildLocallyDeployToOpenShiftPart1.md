---
title: Lab 1 - Build Image and Deploy to OpenShift
workshops: dev_containers_to_openshift
workshop_weight: 11
layout: lab
---

# Lab Overview
Perhaps you are already building container images today and are currently running them locally. This lab will introduce you to the Podman tool for buiding, managing and running container images. For those who are accostomed to using Docker, you will find that Podman command line options are much the same. Once we hae built and tested the image locally, we will then deploy that image to OpenShift.


# Let's get started!

## Let's connect to the workstation terminal.
> <i class="fa fa-terminal"></i> Use your second workstation terminal in the browser, and login to the lab's remote workstation vm:

```bash
$ ssh userX@<workstation.dns>
Login successful.
```

> <i class="fa fa-terminal"></i> Check to see what version of Podman is running

```bash
$ podman version
```

> <i class="fa fa-terminal"></i> View that there are no images currently stored locally

```bash
$ podman images
```

## Let's import the image source project

Let's clone the image source code repository locally 

> <i class="fa fa-terminal"></i> Type the following command to clone the project source code

```bash
$ git clone https://<REPO_LAB_1>
$ cd <REPO_DIR>
$ ls
```

> <i class="fa fa-terminal"></i> Let's review the Dockerfile 

```bash
$ cat Dockerfile
```

You will see that this image is based on the RHEL UBI...

## Let's build the image.

> <i class="fa fa-terminal"></i> Type the following command to build the image using podman

```bash
$ podman build -t XXX
```

## Ok, now let's test the image

> <i class="fa fa-terminal"></i> Type the following command to run the image using podman

```bash
$ podman run XXX &
```

> <i class="fa fa-terminal"></i> Type the following command to test the service

```bash
$ curl https://localhost:8080
```

> <i class="fa fa-terminal"></i> Type the following command to stop the service

```bash
$ podman stop XXX
```

# Let's deploy this image to OpenShift


## Push the image to the XXX Registry

> <i class="fa fa-terminal"></i>
```bash
$ podman ???
```

## Create Deployment for OpenShift

Now go back to the first workstation terminal in the browser

> https://XXXX

We will now create a new project in OpenShift to deploy our image to.

```bash
$ oc new-app <REGISTRY>/<IMAGE>
```

OpenShift will now create a DeploymentConfig and instantiate a new pod to run the container image. Let's check to see that the pod starts up successfully.

```bash
$ oc get pods -w
```

Once the container is running inside the pod, we now need to create a route to expose the service. 
```bash
$ oc expose <SERVICE>
$ oc get routes
```

Now let's test the application running in openshift

```bash
$ curl <ROUTE>
```


# Summary
Congratulations, you have successfully built and deployed a new container image onto OpenShift.

{{< importPartial "footer/footer.html" >}}
