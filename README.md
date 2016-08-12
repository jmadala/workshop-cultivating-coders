## Cultivating Coders Tutorial Guide

## Prerequisites:
* **Github:** This workshop requires an account on a git-based source code repository such as GitHub or BitBucket. To open a free account on GitHub go to this url: http://www.github.com.
* **DockerHub:** It is helpful to have an account on DockerHub. You can open a free account on DockerHub at this url: http://www.dockerhub.com.
* **Command Line Tools:** In order use your Command Line Interface (CLI), you must install the Openshift Origin Client Tools in your path. The binaries for Linux, Windows and OS X can be found here: https://github.com/openshift/origin/releases/tag/v1.2.0. **Please see Advanced Concepts in this guide for more detailed information.**

## Your First Application

We will start working with Digital Garage through the web-based console, allowing us to perform tasks via a browser. About **80%** of Digital Garage's features are available via both the Web-Console and the CLI. More advanced features are only available through the CLI. 

**Working with the Web Console**

Open a browser window (**preferably in Google Chrome**) and go to the following URL: https://thedigitalgarage.io:8443 by right clicking on the link and have the browser open the page in a new tab or new browser window

[insert image of login screen]

Please register and follow the steps to verify your account and create your Digital Garage user profile. 

Since this is the first time you will be making a project on Digital Grage, you will be taken directly to the “Select Image or Template" screen. 

[insert image of a clean "Select Image of Template" screen]

Upon future logins, once you have a roster of projects in your account, you will instead be taken to a list of projects associated with your user account. For now, since you do not have a project, let's create one.

Click the orange **Add Project** button (the folder with a "+" symbol). The next screen will ask you for the name, display name and description of the project. 

*** NOTE: For the name of the project, please be sure to create a unique name such as {username}-first-project to avoid naming conflicts with other users.

Projects are a top level organizational concept. A project allows a community of users (or a user) to organize and manage their content in isolation from other communities. Each project has its own resources, policies (who can or cannot perform actions), and constraints (this project is allowed this much quota, etc.). Projects act as a “wrapper” around all the application services and endpoints you (or your teams) are using for your work.

Click on the **First Project** project to view the operations that you can perform on this specific project. After you click on **First Project**, you will be presented with the **Project Overview** page:

![overview]

[overview]: https://raw.githubusercontent.com/thedigitalgarage/workshop-cultivating-coders/master/project-overview.png

This page will list all of the services and deployments you have running as part of this project. For this example, we do not yet have any pods or services running. 

Let’s change that. 

Click the **Add to Project** button to display a list of applications available for install. You will see the following screen:

![catalog]

[catalog]: https://raw.githubusercontent.com/thedigitalgarage/workshop-cultivating-coders/master/add-to-project.png

Click the **Browse** button and select **Quickstart** from the dropdown menu to filter the list to Quickstart apps. Now, find the **nodejs-mongodb-example** applicaton. Click the card to select. You will see this screen:

![template]

[template]: https://raw.githubusercontent.com/thedigitalgarage/workshop-cultivating-coders/master/template.png

Take a moment to review the options in the Quick Start template. You have the ability to change any of these options but, for now, just choose the defaults by clicking **Create**.

A cofnirmation page will appear with some information about your project and the resources you have just added. You will also see a section detailing how to create a **GitHub webhook**. We will cover webhooks later. For now just click **Continue to Project Overview**. You will see the following screen:

![pods]

[pods]: https://raw.githubusercontent.com/thedigitalgarage/workshop-cultivating-coders/master/po_w_pods.png


As resources are deployed to Digital Garage, the status circle will change. When the circles are green, you know your application has been deployed. It should take about 4 minutes to fully deploy this project. Once completed, click the link titled: **NODEJS-MONGODB-EXAMPLE**. 

__Congratulations! You have just deployed your first application, "Hello World" on the Digital Garage!__

We will be explainging the concepts behind all these actions in due time. For now, we just wanted to give you a taste of how quick and easy it is to deploy a project on Digital Garage.

Feel free to go ahead and play around with the web console to get familiar with the various functionalities. During this workshop, we will be using both the web console command line tools. 

So, about those command line tool...


## Advanced Concepts

### Command Line

To get started with the CLI, we need to:
1. Download the appropriate files for your work machine
2. Extract the oc file
3. Put it in a directory in your path (or you can add the extraction location to your path) 

The necessary files can be found here: https://github.com/openshift/origin/releases/tag/v1.2.0 (scroll to the bottom)

Download the file appropriate for your machine. Then, go to your downloads folder, open the blue folder titled openshift-origin-client-tools... Inside the flder you will see a file simply titled "oc". This is your oc exec file that you will place in a designated folder. We suggest you place this file under **home/opt/bin**. 

Once you complete extracting and moving the file, from the CL, you should be able to:`
```
$ oc version 
oc v1.1.0.1
kubernetes v1.1.0-origin-1107-g4c8e6f4
```
Don’t be concerned if the version numbers are greater than the ones listed here. 

Please register and verify your account to create your Digital Garage user profile. You may have to go through a secure security sign in and obtain a token from Digital Garage. Please follow the steps on your screen to do so. 

NOTE: If you already have a ~/.kube/config file on your machine make sure to rename it something else. If you don’t, your command line tools may try to connect to a different machine.

```
$ oc login
OpenShift server [https://localhost:8443]: https://104.36.17.74:8443	
The server uses a certificate signed by an unknown authority.
You can bypass the certificate check, but any data you send to the server could be intercepted by others.
Use insecure connections? (y/n): {y}

Authentication required for https://104.36.17.74:8443 (openshift) 
Username: {YOUR_EMAIL_ADDRESS}
Password: {ANY_PASSWORD}
Login successful.

Using project "YOUR PROJECT NAME".
```

**Congratulations! You are now authenticated to the Digital Garage server. We are working in a project named XXX-first-project.** 

### Deploying a Stand-Alone Docker Image

#### Background: Containers and Pods

Before we start digging in, let's talk a bit about how **containers** (Docker images and instances) and **pods** are related. Since you are in this class, you may be somewhat familiar with how Docker works, but to use Digital Garage, you actualy don’t need to use any Docker commands at all. Instead, you can interact with the platform directly. But, if you'd like more infomration on Docker, there are several reference guides on how containers work in general and on Docker, in particular:

https://docs.docker.com/introduction/understanding­docker/

https://www.codementor.io/docker/tutorial/what­is­docker­tutorial­andrew­baker­oreilly 

http://developerblog.redhat.com/2014/05/15/practical­introduction­to­docker­containers/

In Digital Garage and in Google Kubernetes, the smallest deployable unit is a **pod**. A pod is a group of one or more Docker containers, and they are guaranteed to be on the same host. 

From the documentation:

```
Each pod has its own IP address, therefore owning its entire port space, and containers within pods can share storage. Pods can be "tagged" with one or more labels, which are then used to select and manage groups of pods in a single operation.

Containers within a pod can be from the same Docker image, though this is considered bad practice. The general idea is for a pod to contain a “server” and any auxiliary services you want to run along with that server. Examples of containers you might put in a pod are, an Apache HTTPD server, a log analyzer, and a file service to help manage uploaded files.

```

Kubernetes (and Digital Garage) uses containers and pods throughout its entire architecture. A pod is actually one container with other containers running inside it. The Digital Garage platform also creates a pod to carry out any source build. Whenever you specify a build, a pod is used to hold the build and create the resulting Docker image. We will talk about builds later in this workshop.

For now, let’s look at the description for one of the pods we deployed earlier in our sample application.

```
$ oc get pods

NAME                             READY     STATUS      RESTARTS   AGE
mongodb-1-2mbok                  1/1       Running     0          1h
nodejs-mongodb-example-1-build   0/1       Completed   0          1h
nodejs-mongodb-example-1-w6cdw   1/1       Running     0          1h

$oc describe pod nodejs-mongodb-example-1-w6cdw

Name:				nodejs-mongodb-example-1-w6cdw
Namespace:			jmac-first-project
Image(s):			172.30.14.188:5000/jmac-first-project/nodejs-mongodb-example@sha256:a6235c7f04614cda80620ce827e1ab48ab055437dbe7b778a925bcedbbf5dbaf
Node:				104.36.17.74/104.36.17.74
Start Time:			Mon, 13 Jun 2016 14:30:38 -0600
Labels:				deployment=nodejs-mongodb-example-1,deploymentconfig=nodejs-mongodb-example,name=nodejs-mongodb-example
Status:				Running
Reason:				
Message:			
IP:				10.1.0.15
Replication Controllers:	nodejs-mongodb-example-1 (1/1 replicas created)
Containers:
  nodejs-mongodb-example:
    Container ID:	docker://e102fdd67ce891692164e9ba1c01bb781a3442e23128867506ebd06f9a8dad59
    Image:		172.30.14.188:5000/jmac-first-project/nodejs-mongodb-example@sha256:a6235c7f04614cda80620ce827e1ab48ab055437dbe7b778a925bcedbbf5dbaf
    Image ID:		docker://69c08ef576bfe2acf107d07d40e7de28843c158bcf50100ea413fe4c337760b6
    QoS Tier:
      cpu:	BestEffort
      memory:	Guaranteed
    Limits:
      memory:	512Mi
    Requests:
      memory:		512Mi
    State:		Running
      Started:		Mon, 13 Jun 2016 14:30:43 -0600
    Ready:		True
    Restart Count:	0
    Environment Variables:
      DATABASE_SERVICE_NAME:	mongodb
      MONGODB_USER:		userH7D
      MONGODB_PASSWORD:		EdeFWjvMSOaploCS
      MONGODB_DATABASE:		sampledb
      MONGODB_ADMIN_PASSWORD:	HftJQb2MAHMJBEho
Conditions:
  Type		Status
  Ready 	True 
Volumes:
  default-token-dmjd3:
    Type:	Secret (a secret that should populate this volume)
    SecretName:	default-token-dmjd3
...

** Note: The above output has been truncated in this document for space considerations.
```

Let’s start by doing the simplest thing possible. Let's get a simple Docker image to run on the Digital Garage platform. To do this, we are going to use the official Docker Hub image for Ghost, a popular blogging tool, that has been further curated by Digital Garage. (https://hub.docker.com/_/ghost/).

```
$ oc new-app thedigitalgarage/ghost
--> Found Docker image e1cbb75 (12 weeks old) from Docker Hub for "thedigitalgarage/ghost"
    * An image stream will be created as "ghost:latest" that will track this image
    * This image will be deployed in deployment config "ghost"
    * Port 2368/tcp will be load balanced by service "ghost"
--> Creating resources with label app=ghost ...
    ImageStream "ghost" created
    DeploymentConfig "ghost" created
    Service "ghost" created
--> Success
    Run 'oc status' to view your app.
```
You can check on the status of your deployment via the command line by typing: 

```
$ oc status
```
**or** you can view the status directly in the web console.

Once the service is running, we can expose a route to the service allowing us to view our application in the browser.

```
$ oc expose svc ghost --hostname=ghost.jmac-first-project.apps.thedigitalgarage.io
route "ghost" exposed
```
These are the only commands you need to get a “plain vanilla” Docker image deployed to Digital Garage. This process should work with any Docker image that follows best practices, such as not running as root, having a port EXPOSED, and even just defining a CMD to execute on start. More informtaion on best practices can be found at:

[Docker Best Practices](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/)
[Project Atomic](http://docs.projectatomic.io/container-best-practices/) (scroll down to Section 5)

#### Background: Services

When we ran the new-app command for Ghost ($ oc new-app thedigitalgarage/ghost), Digital Garage actually created several resources behind the scenes in order to handle deploying and running this Docker image. 

First, it made a **service**, that identifies a set of pods that will be proxied and load balanced. Services assign an IP address and a port pair that, when accessed, redirect to the appropriate back end (pods).

We (and you) care about services because they basically act as a proxy/load balancer between your pods and anything that needs to use the pods and is running inside your Digital Garage environment. For example, if you needed more Apache HTTPD servers to handle the load, you could spin up more Apache HTTPD pods behind the service. The incoming requests to the service would not notice anything different except that the service now does a better job handling the requests.

There is a lot more information about services in the online documentation, including the JSON format to make one by hand. 

For our purposes, let’s take a quick look at the description for the service we've created here:

```
$ oc get svc
NAME                     CLUSTER_IP       EXTERNAL_IP   PORT(S)     SELECTOR                           AGE
ghost                    172.30.36.235    <none>        2368/TCP    app=ghost,deploymentconfig=ghost   15m
mongodb                  172.30.222.254   <none>        27017/TCP   name=mongodb                       2h
nodejs-mongodb-example   172.30.38.15     <none>        8080/TCP    name=nodejs-mongodb-example        2h

$ oc describe svc ghost
Name:			ghost
Namespace:		jmac-first-project
Labels:			app=ghost
Selector:		app=ghost,deploymentconfig=ghost
Type:			ClusterIP
IP:			172.30.36.235
Port:			2368-tcp	2368/TCP
Endpoints:		10.1.0.17:2368
Session Affinity:	None
No events.
```

Let’s also get the JSON for the pod so we can see how OpenShift wired them together.

```
$ oc get pods
NAME                             READY     STATUS      RESTARTS   AGE
ghost-1-06kgq                    1/1       Running     0          19m
mongodb-1-2mbok                  1/1       Running     0          2h
nodejs-mongodb-example-1-build   0/1       Completed   0          2h
nodejs-mongodb-example-1-w6cdw   1/1       Running     0          2h

$ oc describe pod ghost-1-06kgq
Name:				ghost-1-06kgq
Namespace:			jmac-first-project
Image(s):			thedigitalgarage/ghost@sha256:e1cbb752c78201c14edff58134b56296049049e53df398abdf088044f791db73
Node:				104.36.17.74/104.36.17.74
Start Time:			Mon, 13 Jun 2016 16:40:57 -0600
Labels:				app=ghost,deployment=ghost-1,deploymentconfig=ghost
Status:				Running
Reason:				
Message:			
IP:				10.1.0.17
Replication Controllers:	ghost-1 (1/1 replicas created)
Containers:
  ghost:
    Container ID:	docker://37983ac3be9ac72520365b91c954ad3fa1e08bba070a91596c4e844566f7244d
    Image:		thedigitalgarage/ghost@sha256:e1cbb752c78201c14edff58134b56296049049e53df398abdf088044f791db73
    Image ID:		docker://8f1b40ab6cff5a94ac68816d700507388cdaea5d3ebfd202a0988aab1534e347
    QoS Tier:
      memory:		BestEffort
      cpu:		BestEffort
    State:		Running
      Started:		Mon, 13 Jun 2016 16:41:04 -0600
    Ready:		True
    Restart Count:	0
    Environment Variables:
Conditions:
  Type		Status
  Ready 	True 
Volumes:
  ghost-volume-1:
    Type:	EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:	
  default-token-dmjd3:
    Type:	Secret (a secret that should populate this volume)
    SecretName:	default-token-dmjd3
    
    ...
```

Notice the service has a selector of deploymentconfig=ghost. Any pod who has a label key “deploymentconfig” whose value is “ghost” will be matched by this service, and that pod will be listed in the service’s endpoint table.

Next, let's learn a bit about Deployment Configurations and Replication Controllers. 

In the web console, click the up arrow next to the pod status indicator. You will see the number of pods scaling to 2. Click it again, and the number of pods will scale to 3. After all of the pods are in running status, click the url (route) in the service to check and see if Ghost is still running.


#### Background: Routes

By default, the new-app command assumes that you **do not** want to expose the new service to the outside world. If you **do** want to expose a service as an HTTP endpoint, you can easily do this with a route. The Digital Garage platform's global router uses the host HTTP header to determine where to send the request. You can optionally define security, such as TLS, for the route.

In our example we have already exposed the route:

```
$ oc expose svc ghost --hostname=ghost.[YOUR PROJECT NAME].apps.thedigitalgarage.io
route "ghost" exposed
``` 
It's as easy as that. Although routes and routing can get very complicated when you are working in a cloud environment, Digital Garage handles all of that complexity for you.

### Deploy Source Code and a Docker Image

#### Background: Source-To-Image (STI)

We have seen how we can deploy a standard, stand-alone Docker image. Now, let’s see how we can work with source code and builds with Docker images. We can use the new-app command to do a simple deploy of code with a Docker image. The RedHat OpenShift and Digital Garage teams have built some Docker images that are enabled for a more generic build mechanism, called STI.

STI is an Open Source project sponsored by Red Hat. It’s goal is to provide a simple, efficient, and stand-alone mechanism to inject source code into a Docker image and produce a Docker image that can run as-is. The Digital Garage platform is STI enabled and can use STI as one of it’s build mechanisms (in addition to Docker build and custom build). A full discussion of STI is beyond the scope of this workshop; however, please read the documentation to learn more about this project.

All of the programming language images in the OpenShift portion of the Docker Hub registry are STI enabled.

Let's start this next example by creating a new project for just this application. 

In either the web console or from the CLI, please create a XXXX-second-project.
```
$ oc new-project YOUR PROJECT NAME
```

Now, let's fork one of the Digital Garage test apps by going to the Digital Garage repository at: https://github.com/thedigitalgarage

Let's use the nodejs example that we used earlier. This time, however, we are **not** going to use the quickstart template. Instead, We are going to use the STI builder image and the github.com source repository from your fork. Fork **thedigitalgarage/nodejs-ex** application code to your personal repository. Later, we will want you to make a change in the code and then rebuild your application. 

For now, let's go back to the web console.

In your new project, click the **Add to Project** button. You will be taken back to this screen:

![catalog]

*** NOTE: The project name will be your new project name.

This time we are going to choose **NodeJS** from the Browse dropdown menu to filter the choices to just a few images.

Choose the **latest** Nodejs builder image (tagged nodejs:0.10). You may recognize the naming convention of a Docker image. That is because the STI builder image is itself a Docker image. The STI image has all of the tools and scripts needed to pull the source files from our Github repository and build our application into a Docker image that will be deployed on the Digital Garage platform.

You will now be taken to the nodejs builder template screen. It will look much like the last template screen with some important differences such as fewer default settings. Click on the **Advanced Configuration Settings**. Now you have several options to configure the build, including environment variables, Github branches and folders, as well as routing for ssl. These are just the most common build options. If something very specific is needed, you can create your own S2I builder with your own options and defaults. For now, let's just give our application a name and provide the URL to our personal Github repository.

```
Name: nodejs-ex-test
Git Repository URL: https://github.com/YOUR_NAME/nodejs-ex.git
```

Under the Routing section, we want to provide a Hostname.

```
Hostname: nodejs-ex-test-jmac-second-project.apps.thedigitalgarage.io
```
We can leave everything else as a default. Click **Create**.

On the confirmation page, pay close attention to the **Webhooks** section. Copy the **Webhook Payload URL** because we are going to use it in a minute. 

Click **Continue to Overview** on the next screen to go to the **Overview** page.

Here, under the service: **TEST-NODEJS-EXAMPLE**, click **View Log**. This will take you to the log file for that Docker container where you can watch for any build errors. Once the build is complete, and you have a pod running, click on the **route url** in the service. `You will see the same "Hello World" application we built earlier.`

###Webhooks

We are now going to configure a **webhook** that will trigger a rolling re-build on Digital Garage whenever we commit changes to our Github repository, or just when certain pre-selected individual firing events are met. Once a new build is completed, old pods will be automatically removed.

1. In your web browser, log into your personal nodejs-ex repository.
2. Click on "Settings" at the top of the page. 
3. Under settings, find the "Webhooks and services" menu item and click.
4. In the Webhooks section, click the "Add Webhook" button.
5. Paste the URL payload you copied earlier to the Payload URL.
6. Disable SSL Verification.
7. Click the radio button for "Send Me Everything"
8. Click "Add Webhook"
9. Your webhook is now active.

Finally, we need to go back into the Github repository and make a small change to the index.html file in order for you to share your template with other memebers of your team or external parties.

###Using a Template

Running several individual commands to create builds, services, routes and objects can be tedious and error prone. You can actually put all of this configuration together into a **template** file which can then be processed to create a full set of services. In a template, you may have parameters for certain values, such as a database username or password, and these can be automatically generated at processing time.

Templates can actually be loaded on the server so they are available for use in your web console when creating a new application. In order to add a template so that anyone with access to the project can also use the template, you will need to download the template to your local file system. 

To do this, you **must** execute the following command to ensure you point to the location of the template file and **replace **mytemplate** with the actual name of your project**:

$ oc create -f application-template-stibuild.json -n **mytemplate**

We will leave the creation of templates for a future (and exciting!) workshop. 

Thank you all for your time. We hope we've shown you how using Digital Garage as your development environemnt can make life easier for you as you build, test and stage your software applications. We look forward to working with you to continuously ensure Digital Garage is responsive to your needs and to strengthening our training process as well.

The Digital Garage Team
