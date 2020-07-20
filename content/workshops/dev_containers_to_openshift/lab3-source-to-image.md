---
title: Lab 3 - Source-to-Image Build Within OpenShift
workshops: dev_containers_to_openshift
workshop_weight: 31
layout: lab
---

# Source-to-Image Lab
This lab will focus on one of the key advantages of developing within OpenShift: the ability to automatically build containers directly from source.

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
$ ./create-lab3-environment.sh
```

## Make sure the environment is correct:

> From the command line, display the current status.

> <i class="fa fa-terminal"></i> Type the following command to show services, deployment configs, build configurations, and active deployments (this will come in handy later):

```bash
$ oc status
```

# Take a look at the project you will be building from source.
> Click on *URL* to see the project in Github/Git***.  Feel free to click on the project files to review the source files if you want.

# Now let's clone the project into your own Git*** repo:
> You need to make your own copy of the project so you can make modifications to it.  Click the "FORK" button in the upper right corner of the project page to create a copy in your own account. You will be asked for your username and password if you haven't logged in already.

# Create a new project:
> With the project now in your own Git*** repo, create new app pointing to git source code:

```bash
$ oc new-app --name=lab3-sample-app https://github.com/RedHatGov/openshift-workshops.git --context-dir=lab3-sample-app
$ oc expose service lab3-sample-app
```
> Note that OpenShift automatically detects the source code type and selects the appropriate builder image.

# Watch build logs
> This is a good time to review the build logs and see exactly what the build process did:

```bash
$ oc get builds
$ oc logs builds/lab3-sample-app
```

# Test service
> Find the URL at which the service is exposed:

```bash
$ oc get routes
```
> Using copy the route into a browser window and review the output.

# Setup build hook

```bash
$ oc get bc/lab3-sample-app -o yaml | grep generic-webhook
name: lab3-sample-app-generic-webhook-secret

$ SECRET=`oc get secrets lab3-sample-app-generic-webhook-secret -o yaml | grep -i key | sed 's/^.*: //' | base64 -d ; echo`

$ oc describe bc/lab3-sample-app | grep "Webhook Generic" -A 1 | sed "s/<secret>/${SECRET}/"
Webhook Generic:
        URL:            https://api.alexocp43.redhatgov.io:6443/apis/build.openshift.io/v1/namespaces/cicd-1/buildconfigs/lab3-sample-app/webhooks/1234abcd5678efgh/generic
```

# In the Git*** project, enable the webhook:
> From the main page of your personal copy of the project, click the "Settings" tab.  Then click the "Webhook" link.  Add in the URL from "oc describe" above, disabling SSL verification. Click "Add webhook" when done.

# Update the project source code
> Modify a file in your project as follows: *TBD*

# Commit change to git repository
> Now commit the change to the project.

# Watch new build (automatic because of hook)
> The change will trigger the webhook, which will automatically begin a source-to-image build.

# View build logs to verify the success of the build

```bash
$ oc get builds
$ oc logs builds/lab3-sample-app
```

# Watch deployment for success

# Test new service
> Find the URL at which the newly updated service is exposed:

```bash
$ oc get routes
```
> Using copy the route into a browser window and review the output.


{{< importPartial "footer/footer.html" >}}
