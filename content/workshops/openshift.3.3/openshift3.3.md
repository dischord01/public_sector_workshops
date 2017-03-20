+++
title = "OpenShift 3.3 Workshop"
banner = "img/banners/banner-1.jpg"
categories = ["OpenShift", "Developers"]
+++


### If we provided you a laptop for this 

We already set all this up for you - so you can skip everything below.


### Install the command line tools for your operating system
The officially supported instructions for installing the oc client via Red Hat are [located here][1].  

TL;DR:

* Download a .tgz [from Red Hat][5] and add to your PATH
(If you don't have a Red Hat account, you can download the CLI tool from [my host here][7])

<!--For the unsupported community version:

* Download a .tgz [from github][2] and add to your PATH
* Mac users can [brew][3]: 'brew install openshift-cli'

-->


### Install git 
We will use a few git commands directly in some of the advanced labs.  Please make sure git is installed on your system.  You can download the [latest version here][4].

### Create a Github account
Create a FREE github account by going [to this link][6] and following their instructions.


[1]: https://docs.openshift.com/enterprise/latest/cli_reference/get_started_cli.html
[2]: https://github.com/openshift/origin/releases
[3]: http://brew.sh/
[4]: http://git-scm.com/downloads
[5]: https://access.redhat.com/downloads/content/290
[6]: https://github.com/join?source=header-home
[7]: http://openshiftdemos.s3.amazonaws.com/index.html

# A Guided Tour of OpenShift Enterprise

## Welcome to OpenShift!
This lab provides a quick tour of the console to help you get familiar with the user interface along with some key terminology we will use in subsequent lab content.  If you are already familiar with the basics of OpenShift you can skip this lab - after making sure you can login.

## Key Terms
We will be using the following terms throughout the workshop labs so here are some basic definitions you should be familiar with.  And you'll learn more terms along the way, but these are the basics to get you started.

* Container - Your software wrapped in a complete filesystem containing everything it needs to run
* Image - We are talking about docker images; read-only and used to create containers
* Pod - One or more docker containers that run together
* Service - Provides a common DNS name to access a pod (or replicated set of pods)
* Project - A project is a group of services that are related logically
* Deployment - an update to your application triggered by a image change or config change
* Build - The process of turning your source code into a runnable image
* BuildConfig - configuration data that determines how to manage your build
* Route - a labeled and DNS mapped network path to a service from outside OpenShift
* Master - The foreman of the OpenShift architecture, the master schedules operations, watches for problems, and orchestrates everything
* Node - Where the compute happens, your software is run on nodes

## Accessing OpenShift
OpenShift provides a web console that allow you to perform various tasks via a web browser.  Additionally, you can utilize a command line tool to perfrom tasks.  Let's get started by logging into both of these and checking the status of the platform.

### Let's Login
> Navigate to the URI provided by your instructor and login with the user/password provided (if there's an icon on the Desktop, just double click that)



{{< figure src="/workshops/openshift.3.3/images/ose-login.png" width="600" title="OpenShift Login Page" >}}


*Login Webpage*

Once logged in you should see your available projects - or a button to create a project if none exist already.

### So this is what an empty project looks like
First let's create a new project to do our workshop work in.  We will use the student number you were given to ensure you don't clash with classmates, so in the steps below replace 'YOUR#' with your student number.

> Click on the "New Project" button and give it a name of demo-YOUR#

> Populate "Display Name" with "demo-YOUR#" and populate "Description" boxes with whatever you like.  And click "Create"

This is going to take you to the next logical step of adding something to the project, but we don't want to do that just yet.

> Click the "demo-YOUR#" link on the top left to goto your project

Don't worry, it's supposed to look empty right now because you currently don't have anything in your project (we'll fix that in the next lab).

### Let's try the command line

> <i class="fa fa-terminal"></i> Open a terminal and login using the same URI/user/password with following command:

```bash
oc login [URI]
```

> <i class="fa fa-terminal"></i> Check to see what projects you have access to:

```bash
$ oc get projects
```

### It looks empty via the command line too

You just created a project using the web console, let's tell the terminal command line tool to use it.

> <i class="fa fa-terminal"></i> Type the following command to use the demo project

```bash
$ oc project demo-YOUR#
```


> <i class="fa fa-terminal"></i> Type the following command to show services, deployment configs, build configurations, and active deployments (this will come in handy later):

```bash
$ oc status
```

## Summary
You should now be ready to get hands-on with our workshop labs.


# BYO docker

## Bring your own docker

It's easy to get started with OpenShift whether that be using our app templates or bringing your existing docker assets.  In this quick lab we will deploy app using an exisiting docker image.  OpenShift will create an image stream for the image as well as deploy and manage containers based on that image.  And we will dig into the details to show how all that works.

### Let's point OpenShift to an existing built docker image


{{< cli A >}}
$ oc new-app kubernetes/guestbook


{{< bq "The output should show something *similar* to below:" >}}

```bash
--> Found docker image a49fe18 (17 months old) from docker Hub for "kubernetes/guestbook"
    * An image stream will be created as "guestbook:latest" that will track this image
    * This image will be deployed in deployment config "guestbook"
    * Port 3000/tcp will be load balanced by service "guestbook"
--> Creating resources with label app=guestbook ...
    ImageStream "guestbook" created
    DeploymentConfig "guestbook" created
    Service "guestbook" created
--> Success
    Run 'oc status' to view your app.
```

{{< web >}}


{{< bq "Click 'Add to Project'" >}}

{{< figure src="/workshops/openshift.3.3/images/ose-lab-s2i-addbutton.png" width="100" >}}


{{< bq "Select the tab for "Deploy Image" from the top options" >}}

{{< figure src="/workshops/openshift.3.3/images/ose-guestbook-deploy-image.png" width="400" >}}


{{< bq "Select the option for 'Image Name' and enter 'kubernetes/guestbook', then click the magnifying glass to the far right to search for the image." >}}

{{< figure src="/workshops/openshift.3.3/images/ose-guestbook-imagename-expand.png" width="600" >}}


{{< bq "Observe default values that are populated in the search results:" >}}

{{< figure src="/workshops/openshift.3.3/images/ose-guestbook-create-1.png" width="600" >}}
{{< figure src="/workshops/openshift.3.3/images/ose-guestbook-create-2.png" width="600" >}}


{{< bq "Scroll to the bottom and click Create" >}}

{{< accord-end >}}




### We can browse our project details with the command line
> <i class="fa fa-terminal"></i> Try typing the following to see what is available to 'get':

```bash
$ oc get
```

> <i class="fa fa-terminal"></i> Now let's look at what our image stream has in it

```bash
$ oc get is
```

```bash
$ oc describe is/guestbook
```

<i class="fa fa-info-circle"></i> An image stream can be used to automatically perform an action, such as updating a deployment, when a new image, such as a new version of the guestbook image, is created.

> <i class="fa fa-terminal"></i> The app is running in a pod, let's look at that

```bash
$ oc describe pods
```

### We can see those details using the web console too
Let's look at the image stream.  


{{< bq "Click on 'Builds -> Images'" >}}


This shows a list of all image streams within the project.  


{{< bq "Now click on the guestbook image stream" >}}


You should see something similar to this:

{{ figure src="/workshops/openshift.3.3/images/ose-guestbook-is.png" width="600" >}}


### Does this guestbook do anything?
Good catch, your service is running but there is no way for users to access it yet.  We can fix that with the web console or the command line, you decide which you'd rather do from the steps below.

<div class="panel-group" id="accordion" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingOne">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordion" href="#collapseOne" aria-expanded="true" aria-controls="collapseOne">
          Web Console Steps
        </a>
      </div>
    </div>
    <div id="collapseOne" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingOne">
      <div class="panel-body">

<blockquote>
To expose via the web console, click on "Overview" to get to this view:
</blockquote>
<img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-guestbook-noroute.png" width="600"/><br/>

<p>Notice there is no exposed route </p>

<blockquote>
Click on the "Create Route" link
</blockquote>

<img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-guestbook-createroute.png" width="600"/><br/>

<p>This is where you could specify route parameters, but we will just use the defaults.</p>

<blockquote>
Click "Create"
</blockquote>

      </div>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingTwo">
      <div class="panel-title">
        <a class="collapsed" role="button" data-toggle="collapse" data-parent="#accordion" href="#collapseTwo" aria-expanded="false" aria-controls="collapseTwo">
          Alternatively: CLI Steps
        </a>
      </div>
    </div>
    <div id="collapseTwo" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingTwo">
      <div class="panel-body">

<blockquote>
<i class="fa fa-terminal"></i> In the command line type this:
</blockquote>

{% highlight csh %}
$ oc expose service guestbook
{% endhighlight %}

      </div>
    </div>
  </div>
</div>

<i class="fa fa-info-circle"></i> You can also create secured HTTPS routes, but that's an advanced topic for a later lab

### Test out the guestbook webapp
Notice that in the web console overview, you now have a URL in the service box.  There is no database setup, but you can see the webapp running by clicking the route you just exposed.

> Click the link in the service box


You should see:
<img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-guestbook-app.png" width="600"/><br/>


### Good work, let's clean this up
> <i class="fa fa-terminal"></i> Let's clean up all this to get ready for the next lab:

{% highlight csh %}
$ oc delete all --all
{% endhighlight %}


## Summary
In this lab you've deployed an example docker image, pulled from docker hub, into a pod running in OpenShift.  You exposed a route for clients to access that service via thier web browsers.  And you learned how to get and describe resources using the command line and the web console.  Hopefully, this basic lab also helped to get you familiar with using the CLI and navigating within the web console.

# Deploying an App with S2I

## Source to Image (S2I)
One of the useful components of OpenShift is its source-to-image capability.  S2I is a framework that makes it easy to turn your source code into runnable images.  The main advantage of using S2I for building reproducible docker images is the ease of use for developers.  You'll see just how simple it can be in this lab.

### Let's build a node.js web server using S2I
We can do this either via the command line (CLI) or the web console.  You decide which you'd rather do and follow the steps below:

<div class="panel-group" id="accordionA" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingAOne">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordionA" href="#collapseAOne" aria-expanded="true" aria-controls="collapseAOne">
          CLI Steps
        </a>
      </div>
    </div>
    <div id="collapseAOne" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingAOne">
      <div class="panel-body">

<blockquote>
<i class="fa fa-terminal"></i> Goto the terminal and type the following:
</blockquote>
{% highlight csh %}
$ oc new-app --name=dc-metro-map https://github.com/dudash/openshift-workshops.git --context-dir=dc-metro-map
$ oc expose service dc-metro-map
{% endhighlight %}

<i class="fa fa-info-circle"></i> OpenShift auotmatically detected the source code type and selected the nodejs builder image

      </div>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingATwo">
      <div class="panel-title">
        <a class="collapsed" role="button" data-toggle="collapse" data-parent="#accordionA" href="#collapseATwo" aria-expanded="false" aria-controls="collapseATwo">
          Web Console Steps
        </a>
      </div>
    </div>
    <div id="collapseATwo" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingATwo">
      <div class="panel-body">

<blockquote>
Click "Add to Project"
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-s2i-addbutton.png" width="100"/></p>

<blockquote>
Click "Browse Catalog" and filter for nodejs, then click the nodejs:0.10 builder image
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-s2i-filternode.png" width="600"/></p>

<blockquote>
Fill out the boxes to look as follows:
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-s2i-addtoproject.png" width="600"/></p>
<p>
Notes: You will need to click to expand the "advanced options"<br/>
The github repository URL is: https://github.com/dudash/openshift-workshops.git<br/>
The github context-dir is: dc-metro-map<br/>
</p>

<blockquote>
Scroll to the bottom and click "Create"
</blockquote>

      </div>
    </div>
  </div>
</div>

### Check out the build details
We can see the details of what the S2I builder did.  This can be helpful to diagnose issues if builds are failing.

<i class="fa fa-magic"></i> TIP: For a node.js app, running "npm shrinkwrap" is a good practice to perform on your branch before releasing changes that you plan to build/deploy as an image with S2I

<div class="panel-group" id="accordionB" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingBOne">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordionB" href="#collapseBOne" aria-expanded="true" aria-controls="collapseBOne">
          CLI Steps
        </a>
      </div>
    </div>
    <div id="collapseBOne" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingBOne">
      <div class="panel-body">

<blockquote>
<i class="fa fa-terminal"></i> Goto the terminal and type the following:
</blockquote>

{% highlight csh %}
$ oc get builds
{% endhighlight %}

In the output, note the name of your build and use it to see the logs with:

{% highlight csh %}
$ oc logs builds/[BUILD_NAME]
{% endhighlight %}

The console will print out the full log for your build.  Note, you could pipe this to more or less for easier viewing in the CLI.

      </div>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingBTwo">
      <div class="panel-title">
        <a class="collapsed" role="button" data-toggle="collapse" data-parent="#accordionB" href="#collapseBTwo" aria-expanded="false" aria-controls="collapseBTwo">
          Web Console Steps
        </a>
      </div>
    </div>
    <div id="collapseBTwo" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingBTwo">
      <div class="panel-body">

<blockquote>
Click on "Builds" and then click on "Builds"
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-s2i-builds.png" width="300"/></p>

<blockquote>
Click on the "dc-metro-map" link
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-s2i-metromapbuild.png" width="300"/></p>

<blockquote>
Click on the "View Log" tab to see the details of your latest build
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-s2i-metromapbuilds.png" width="500"/></p>

You should see a log output similar to the one below:
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-s2i-metromapbuildlog.png" width="500"/></p>

      </div>
    </div>
  </div>
</div>

### See the app in action
Let's see this app in action!

<div class="panel-group" id="accordionC" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingCOne">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordionC" href="#collapseCOne" aria-expanded="true" aria-controls="collapseCOne">
          CLI Steps
        </a>
      </div>
    </div>
    <div id="collapseCOne" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingCOne">
      <div class="panel-body">
<blockquote>
<i class="fa fa-terminal"></i> Goto the terminal and type the following:
</blockquote>

{% highlight csh %}
$ oc get routes
{% endhighlight %}

<blockquote>
Copy the HOST/PORT and paste into your favorite web browser
</blockquote>

      </div>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingCTwo">
      <div class="panel-title">
        <a class="collapsed" role="button" data-toggle="collapse" data-parent="#accordionC" href="#collapseCTwo" aria-expanded="false" aria-controls="collapseCTwo">
          Web Console Steps
        </a>
      </div>
    </div>
    <div id="collapseCTwo" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingCTwo">
      <div class="panel-body">

<blockquote>
Click on Overview
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-s2i-overview.png" width="100"/></p>

<blockquote>
Click the URL that is listed in the dc-metro-map header
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-s2i-dcmetromapsvc.png" width="600"/></p>

      </div>
    </div>
  </div>
</div>

The app should look like this in your web browser:

<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-s2i-apprunning.png" width="500"/></p>

Clicking the checkboxes will toggle on/off the individual metro stations on each colored line.  A numbered icon indicates there is more than one metro station in that area and they have been consolidated - click the number or zoom in to see more.

## Summary
In this lab we deployed a sample application using source to image.  This process built our code and wrapped that in a docker image.  It then deployed the image into our OpenShift platform in a pod and exposed a route to allow outside web traffic to access our application.  In the next lab we will look at some details of this app's deployment and make some changes to see how OpenShift can help to automate our development processes.

[1]: https://docs.openshift.com/enterprise/latest/dev_guide/new_app.html


# Developing and Managing Your Application

## Developing and managing an application in OpenShift
In this lab we will explore some of the common activities undertaken by developers working in OpenShift.  You will become familiar with how to use environment variables, secrets, build configurations, and more.  Let's look at some of the basic things a developer might care about for a deployed app.

### Setup
From the previous lab you should have the DC Metro Maps web app running in OpenShift.  

<i class="fa fa-warning"></i> **Only if you don't already have it running already, add it with the following steps.**

> <i class="fa fa-terminal"></i> Goto the terminal and type these commands:

{% highlight csh %}
$ oc new-app --name=dc-metro-map https://github.com/dudash/openshift-workshops.git --context-dir=dc-metro-map
$ oc expose service dc-metro-map
{% endhighlight %}


### See the app in action and inspect some details
There is no more ambiguity or confusion about where the app came from.  OpenShift provides traceability for your running deployment back to the docker image and the registry it came from, as well as (for images built by openshift) back to the exact source code branch and commit.  Let's take a look at that.

<div class="panel-group" id="accordionA" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingAOne">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordionA" href="#collapseAOne" aria-expanded="true" aria-controls="collapseAOne">
          CLI Steps
        </a>
      </div>
    </div>
    <div id="collapseAOne" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingAOne">
      <div class="panel-body">

<blockquote>
<i class="fa fa-terminal"></i> Goto the terminal and type the following:
</blockquote>
{% highlight csh %}
$ oc status
{% endhighlight %}

This is going to show the status of your current project.  In this case it will show the dc-metro-map service (svc) with a nested deployment config (dc) along with some more info that you can ignore for now.  

<br/><br/><i class="fa fa-info-circle"></i>  A deployment in OpenShift is a replication controller based on a user defined template called a deployment configuration <br/><br/>

The dc provides us details we care about to see where our application image comes from, so let's check it out in more detail.

<blockquote>
<i class="fa fa-terminal"></i> Type the following to find out more about our dc:
</blockquote>
{% highlight csh %}
$ oc describe dc/dc-metro-map
{% endhighlight %}

Notice under the template section it lists the containers it wants to deploy along with the path to the container image.

<br/><br/><i class="fa fa-info-circle"></i> There are a few other ways you could get to this information.  If you are feeling adventurous, you might want to describe the replication controller (oc describe rc -l app=dc-metro-map), the image stream (oc describe is -l app=dc-metro-map) or the running pod itself (oc describe pod -l app=dc-metro-map).<br/><br/>

Because we built this app using S2I, we get to see the details about the build - including the container image that was used for building the source code.  So let's find out where the image came from.  Here are the steps to get more information about the build configuration (bc) and the builds themselves.

<blockquote>
<i class="fa fa-terminal"></i> Type the following to find out more about our bc:
</blockquote>
{% highlight csh %}
$ oc describe bc/dc-metro-map
{% endhighlight %}

Notice the information about the configuration of how this app gets built.  In particular look at the github URL, the webhooks you can use to automatically trigger a new build, the docker image where the build runs inside of, and the builds that have been completed.  New let's look at one of those builds.

<blockquote>
<i class="fa fa-terminal"></i> Type the following:
</blockquote>
{% highlight csh %}
$ oc describe build/dc-metro-map-1
{% endhighlight %}

This shows us even more about the deployed container's build and source code including exact commit GUID for this build.  We can also can see the commit's author, and the commit message.  You can inspect the code by opening a web browser and pointing it to: https://github.com/dudash/openshift-workshops/commit/[COMMIT_GUID]

      </div>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingATwo">
      <div class="panel-title">
        <a class="collapsed" role="button" data-toggle="collapse" data-parent="#accordionA" href="#collapseATwo" aria-expanded="false" aria-controls="collapseATwo">
          Web Console Steps
        </a>
      </div>
    </div>
    <div id="collapseATwo" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingATwo">
      <div class="panel-body">

<blockquote>
Click "Overview"
</blockquote>
<blockquote>
Check out the details within the deployment (above and to the right of the Pods circle). 
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-devman-deployment-shortcut.png" width="200"/></p>

Within the deployment for the dc-metro-map is a container summary that shows both the GUID for the image and the GUID for the git branch.
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-devman-containertracibility.png" width="500"/></p>

<blockquote>
Click on the link next to "Image:"
</blockquote>
Here are the details of the image stream for this deployment.
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-devman-dcmteroimagestream.png" width="500"/></p>

<i class="fa fa-info-circle"></i> If you hover over the shortened image GUID or edit the image stream you can see the full GUID.<br/><br/>

<blockquote>
Click "Builds" and then Builds to get back to the build summary
</blockquote>

<blockquote>
Click "#1" to see the build details
</blockquote>
Because we built this app using S2I, we get to see the details about the build - including the container image that was used for building the source code.  Note that you can kick-off a rebuild here if something went wrong with the initial build and you'd like to attempt it again.
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-devman-buildsummary.png" width="500"/></p>

<blockquote>
Click "Overview" and then the deployment detail link to get back to the deployment summary again
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-devman-deployment-shortcut.png" width="200"/></p>
Notice that next to the build # you can see the comment from the last commit when the build was started.  And you can see the that commit's author.  You can click that commit GUID to be taken to the exact version of the source code that is in this deployed application.
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-devman-commitmsg.png" width="400"/></p>

      </div>
    </div>
  </div>
</div>


### Pod logs
In the S2I lab we looked at a build log to inspect the process of turning source code into an image.  Now let's inspect the log for a running pod - in particular let's see the web application's logs.

<div class="panel-group" id="accordionB" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingBOne">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordionB" href="#collapseBOne" aria-expanded="true" aria-controls="collapseBOne">
          CLI Steps
        </a>
      </div>
    </div>
    <div id="collapseBOne" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingBOne">
      <div class="panel-body">

<blockquote>
<i class="fa fa-terminal"></i> Goto the terminal and type the following:
</blockquote>
{% highlight csh %}
$ oc get pods
{% endhighlight %}

This is going to show basic details for all pods in this project (including the builders).  Let's look at the log for the pod running our application.  Look for the POD NAME that that is "Running" you will use it below.

<blockquote>
<i class="fa fa-terminal"></i> Goto the terminal and type the following (replacing the POD ID with your pod's ID):
</blockquote>
{% highlight csh %}
$ oc logs [POD NAME]
{% endhighlight %}

You will see in the output details of your app starting up and any status messages it has reported since it started.

<br/><br/><i class="fa fa-info-circle"></i> You can see more details about the pod itself with 'oc describe pods/[POD NAME]'

      </div>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingBTwo">
      <div class="panel-title">
        <a class="collapsed" role="button" data-toggle="collapse" data-parent="#accordionB" href="#collapseBTwo" aria-expanded="false" aria-controls="collapseBTwo">
          Web Console Steps
        </a>
      </div>
    </div>
    <div id="collapseBTwo" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingBTwo">
      <div class="panel-body">

<blockquote>
Click on "Applications" and then click on "Pods"
</blockquote>
This is going to show basic details for all pods in this project (including the builders).
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-devman-allpods.png" width="500"/></p>
Next let's look at the log for the pod running our application.

<blockquote>
Click the pod that starts with "dc-metro-map-" and has a status of Running
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-devman-poddetails.png" width="500"/></p>
Here you see the status details of your pod as well as its configuration.  Take a minute here and look at what details are available.

<blockquote>
Click the "Logs" button
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-devman-podslogs.png" width="500"/></p>
Now you can see in the output window the details of your app starting up and any status messages it has reported since it started.

      </div>
    </div>
  </div>
</div>


### How about we set some environment variables?
Whether it's a database name or a configuration variable, most applications make use of environment variables.  It's best not to bake these into your containers because they do change and you don't want to rebuild an image just to change an environment variable.  Good news!  You don't have to.  OpenShift let's you specify environment variables in your deployment configuration and they get passed along through the pod to the container.  Let's try doing that.

<div class="panel-group" id="accordionC" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingCOne">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordionC" href="#collapseCOne" aria-expanded="true" aria-controls="collapseCOne">
          CLI Steps
        </a>
      </div>
    </div>
    <div id="collapseCOne" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingCOne">
      <div class="panel-body">

Let's have a little fun.  The app has some easter eggs that get triggered when certain env vars are set to 'true'.
<blockquote>
<i class="fa fa-terminal"></i> Goto the terminal and type the following:
</blockquote>
{% highlight csh %}
$ oc env dc/dc-metro-map -e BEERME=true
{% endhighlight %}

{% highlight csh %}
$ oc get pods -w
{% endhighlight %}

Due to the deployment config strategy being set to "Rolling" and the "ConfigChange" trigger being set, OpenShift auto deployed a new pod as soon as you updated with the env variable.  If you were quick enough you saw this happening with the get pods command

<blockquote>
<i class="fa fa-terminal"></i> Type Ctrl+C to stop watching the pods
</blockquote>

<i class="fa fa-info-circle"></i> You can set env variables across all deployment configs with 'dc --all' instead of specifying a specifc config

      </div>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingCTwo">
      <div class="panel-title">
        <a class="collapsed" role="button" data-toggle="collapse" data-parent="#accordionC" href="#collapseCTwo" aria-expanded="false" aria-controls="collapseCTwo">
          Web Console Steps
        </a>
      </div>
    </div>
    <div id="collapseCTwo" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingCTwo">
      <div class="panel-body">

<blockquote>
Click on "Applications" and then click on "Deployments"
</blockquote>
This is going to show basic details for all deployment configurations in this project

<blockquote>
Click the "dc-metro-map" deployment config
</blockquote>
There are a lot of details here, feel free to check them out and ask questions, but we are here to set some new environment variables.  

<blockquote>
Click the Environment tab next to the Details tab .
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-devman-deployconfigdetails-config.png" width="500"/></p>
This opens up a tab with the environment variables for this deployment config.

<blockquote>
Add an environment variable with the name BEERME and a value of 'true'
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-devman-deployconfigdetails-populated.png" width="500"/></p>

<blockquote>
Click "Save".  And go back to the summary view by clicking "Overview" on the left menu bar
</blockquote>
If you are quick enough you will see a new pod spin up and an the old pod spin down.  This is due to the deployment config strategy being set to "Rolling" and having a "ConfigChange" trigger, OpenShift auto deployed a new pod as soon as you updated with the env variable.
      </div>
    </div>
  </div>
</div>

With the new environment variables set the app should look like this in your web browser (with beers instead of busses):

<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-devman-beerme.png" width="500"/></p>


### What about passwords and private keys?
Environment variables are great, but sometimes we don't want sensitive data exposed in the environment.  We will get into using **secrets** later when you do the lab: [Keep it Secret, Keep it Safe][2]


### Getting into a pod
There are situations when you might want to jump into a running pod, and OpenShift lets you do that pretty easily.  We set some environment variables and secrets in this lab, let's jump onto our pod to inspect them.  

<div class="panel-group" id="accordionD" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingDOne">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordionD" href="#collapseDOne" aria-expanded="true" aria-controls="collapseDOne">
          CLI Steps
        </a>
      </div>
    </div>
    <div id="collapseDOne" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingDOne">
      <div class="panel-body">

<blockquote>
<i class="fa fa-terminal"></i> Goto the terminal and type the following:
</blockquote>
{% highlight csh %}
$ oc get pods
{% endhighlight %}

Find the pod name for your Running pod

{% highlight csh %}
$ oc exec -it [POD NAME] /bin/bash
{% endhighlight %}
 
You are now interactively attached to the container in your pod.  Let's look for the environment variables we set:

{% highlight csh %}
$ env | grep BEER
{% endhighlight %}

That should return the **BEERME=true** matching the value that we set in the deployment config.

{% highlight csh %}
$ exit
{% endhighlight %}

      </div>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingDTwo">
      <div class="panel-title">
        <a class="collapsed" role="button" data-toggle="collapse" data-parent="#accordionD" href="#collapseDTwo" aria-expanded="false" aria-controls="collapseDTwo">
          Web Console Steps
        </a>
      </div>
    </div>
    <div id="collapseDTwo" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingDTwo">
      <div class="panel-body">

<blockquote>
Click on "Applications" and then click on "Pods"
</blockquote>

<blockquote>
Click the pod that starts with "dc-metro-map-" and has a status of Running
</blockquote>

<blockquote>
Click the "Terminal" button
</blockquote>

<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-devman-podterminal.png" width="500"/></p>
Let's look for the environment variables we set:

<blockquote>
Inside the web page's terminal type: 'env | grep BEER'
</blockquote>
That should return the **BEERME=true** matching the value that we set in the deployment config.

      </div>
    </div>
  </div>
</div>

### Good work, let's clean this up
> <i class="fa fa-terminal"></i> Let's clean up all this to get ready for the next lab:

{% highlight csh %}
$ oc delete all -l app=dc-metro-map
{% endhighlight %}
  
## Summary
In this lab you've seen how to trace running software back to its roots, how to see details on the pods running your software, how to update deployment configurations, how to inspect logs files, how to set environment variables consistently across your environment, and how to interactively attach to running containers.  All these things should come in handy for any developer working in an OpenShift platform.

To dig deeper in to details behind the steps you performed in this lab, check out the OSE [developer's guide][1].

[1]: https://docs.openshift.com/enterprise/3.1/dev_guide/index.html
[2]: ./workshop-lab-secrets.html


# Webhooks and Rollbacks

## Build Triggers, Webhooks and Rollbacks - Oh My!
Once you have an app deployed in OpenShift you can take advantage of some continuous capabilities that help to enable DevOps and automate your management process.  We will cover some of those in this lab: Build triggers, webhooks, and rollbacks.


### A bit of configuration
We are going to do some integration and coding with an external git repository.  For this lab we are going to use github, if you don't already have an account, [you can create one here][3].

OK, let's fork the dc-metro-map app from **my** account into **your** github account.  Goto [https://github.com/dudash/openshift-workshops/][4] and look to the top right for the "Fork" button.

<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-rollbacks-fork.png" width="400"/></p>

> Click the "Fork" button

Github should redirect you to the newly created fork of the source code.


### Build Trigger / Code Change Webhook
When using S2I there are a few different things that can be used to [trigger][1] a rebuild of your source code.  The first is a configuration change, the second is an image change, and the last (which we are covering here) is a webhook.  A webhook is basically your git source code repository telling OpenShift that the code we care about has changed.  Let's set that up for our project now to see it in action.

Jump back to your OpenShift web console and let's add the webapp to our project.  You should know how to do this from previous lab work, but this time point to *your* github URL for the source code.  If you need a refresher expand the box below.

<div class="panel-group" id="accordOpt" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headinOpt">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordOpt" href="#collOpt" aria-expanded="false" aria-controls="collOpt">
          Web Console Steps
        </a>
      </div>
    </div>
    <div id="collOpt" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headinOpt">
      <div class="panel-body">

<blockquote>
Click the "Add to Project" button
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-rollbacks-add-to-project.png" width="100"/></p>
<blockquote>
Select the "Browse Catalog" tab and search for the nodejs:0.10 builder image.
</blockquote>
<blockquote>
Fill out the boxes to point to the fork and context dir
</blockquote>

<p>
Notes: You will need to click to expand the "advanced options"<br/>
The github repository URL is: https://github.com/YOUR_ACCOUNT/openshift-workshops.git<br/>
The github context-dir is: /dc-metro-map<br/>
</p>

      </div>
    </div>
  </div>
</div>

The node.js builder template creates a number of resources for you, but what we care about right now is the build configuration because that contains the webhooks.  So to get the URL:

<div class="panel-group" id="accordion" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingOne">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordion" href="#collapseOne" aria-expanded="true" aria-controls="collapseOne">
          CLI Steps
        </a>
      </div>
    </div>
    <div id="collapseOne" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingOne">
      <div class="panel-body">

<blockquote>
<i class="fa fa-terminal"></i> Goto the terminal and type the following:
</blockquote>
{% highlight csh %}
$ oc describe bc/dc-metro-map | grep -i webhook
{% endhighlight %}

<blockquote>
Copy the Generic webhook to the clipboard
</blockquote>

      </div>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingTwo">
      <div class="panel-title">
        <a class="collapsed" role="button" data-toggle="collapse" data-parent="#accordion" href="#collapseTwo" aria-expanded="false" aria-controls="collapseTwo">
          Web Console Steps
        </a>
      </div>
    </div>
    <div id="collapseTwo" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingTwo">
      <div class="panel-body">
        
<blockquote>
Click on "Builds" and then click on "Builds"
</blockquote>
This is going to show basic details for all build configurations in this project
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-rollbacks-buildconfigs.png" width="500"/></p>

<blockquote>
Click the "dc-metro-map" build config
</blockquote>
You will see the summary of builds using this build config
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-rollbacks-buildconfigsummary.png" width="500"/></p>

<blockquote>
Click the "Configuration" tab (next to the active Summary tab)
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-devman-deployconfigconfig.png" width="500"/></p>
Now you can see the various configuration details including the Github specific and Generic webhook URLs.

<blockquote>
Copy the Generic webhook to the clipboard
</blockquote>

      </div>
    </div>
  </div>
</div>

<br/>

> Now switch back over to github 

<div class="panel-group" id="accordionC" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingCOne">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordionC" href="#collapseCOne" aria-expanded="true" aria-controls="collapseCOne">
          Github Steps
        </a>
      </div>
    </div>
    <div id="collapseCOne" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingCOne">
      <div class="panel-body">

Let's put the webhook URL into the repository. At the main page for this repository (the fork), you should see a tab bar with code, pull requests, pulse, graphs, and settings.

<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-rollbacks-settings.png" width="400"/></p>

<blockquote>
Click the "Settings" tab
</blockquote>

Now you will see a vertical list of settings groups.<br/><br/>

<blockquote>
Click the "Webhooks & services" item
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-rollbacks-githubwebhooks.png" width="600"/></p>

<blockquote>
Click the "Add webhook" button
</blockquote>
<blockquote>
Paste in the URL you copied
</blockquote>
<blockquote>
Disable SSL verification by clicking the button
</blockquote>
<i class="fa fa-info-circle"></i> You can learn how to setup SSH in the secrets lab<br/><br/>

<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-rollbacks-githubwebhooks-add.png" width="600"/></p>

<blockquote>
Click the button to "Add webhook"
</blockquote>

      </div>
    </div>
  </div>
</div>

Good work!  Now any "push" to the forked repository will send a webhook that triggers OpenShift to: re-build the code and image using s2i, and then perform a new pod deployment.  In fact Github should have sent a test trigger and OpenShift should have kicked off a new build already.


### Deployment Triggers
<i class="fa fa-info-circle"></i> In addition to setting up triggers for rebuilding code, we can setup a different type of [trigger][2] to deploy pods.  Deployment triggers can be due to a configuration change (e.g. environment variables) or due to an image change.  This powerful feature will be covered in one of the advanced labs.


### Rollbacks
Well, what if something isn't quite right with the latest version of our app?  Let's say some feature we thought was ready for the world really isn't - and we didn't figure that out until after we deployed it.  No problem, we can roll it back with the click of a button.  Let's check that out:

<div class="panel-group" id="accordionB" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingBOne">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordionB" href="#collapseBOne" aria-expanded="true" aria-controls="collapseBOne">
          CLI Steps
        </a>
      </div>
    </div>
    <div id="collapseBOne" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingBOne">
      <div class="panel-body">

<blockquote>
<i class="fa fa-terminal"></i> Goto the terminal and type the following:
</blockquote>
{% highlight csh %}
$ oc rollback dc-metro-map-1
$ oc get pods -w
{% endhighlight %}

      </div>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingBTwo">
      <div class="panel-title">
        <a class="collapsed" role="button" data-toggle="collapse" data-parent="#accordionB" href="#collapseBTwo" aria-expanded="false" aria-controls="collapseBTwo">
          Web Console Steps
        </a>
      </div>
    </div>
    <div id="collapseBTwo" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingBTwo">
      <div class="panel-body">

<blockquote>
Click on "Applications" and then click on "Deployments"
</blockquote>
This is going to show basic details for all deployment configurations in this project

<blockquote>
Click the "dc-metro-map" deployment config
</blockquote>
Toward the bottom of the screen you will see a table of deployments using this deployment config
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-rollbacks-deploymentconfigsummary.png" width="600"/></p>

<blockquote>
In the Deployments table click the #1
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-rollbacks-deploymentconfig1.png" width="500"/></p>

<blockquote>
Click the "Rollback button", accept defaults, and click "Rollback" again
</blockquote>

You can go back to the overview page to see your previous deployment spinning down and your new one spinning up.

      </div>
    </div>
  </div>
</div>

OpenShift has done a graceful removal of the old pod and created a new one.  

<i class="fa fa-info-circle"></i> The old pod wasn't killed until the new pod was successfully started and ready to be used.  This is so that OpenShift could continue to route traffic to the old pod until the new one was ready.

<i class="fa fa-info-circle"></i> You can integrate your CI/CD tools to do [rollbacks with the REST API][5].

## Summary
In this lab we saw how you can configure a source code repository to trigger builds with webhooks.  This webhook could come from Github, Jenkins, Travis-CI, or any tool capable of sending a URL POST.  Keep in mind that there are other types of build triggers you can setup.  For example: if a new version of the upstream RHEL image changes.  We also inspected our deployment history and did a rollback of our running deployment to one based on an older image with the click of a button.


[1]: https://docs.openshift.com/enterprise/3.1/dev_guide/builds.html#build-triggers
[2]: https://docs.openshift.com/enterprise/3.1/dev_guide/deployments.html#triggers
[3]: https://github.com/join?source=header-home
[4]: https://github.com/dudash/openshift-workshops/
[5]: https://docs.openshift.com/enterprise/latest/rest_api/openshift_v1.html#create-a-deploymentconfigrollback-2

# Replication and Recovery

## Things will go wrong, and that's why we have replication and recovery
Things will go wrong with your software, or your hardware, or from something out of your control.  But we can plan for that failure, and planning for it let's us minimize the impact.  OpenShift supports this via what we call replication and recovery.

### Replication
Let's walk through a simple example of how the replication controller can keep your deployment at a desired state.  Assuming you still have the dc-metro-map project running we can manually scale up our replicas to handle increased user load.

<div class="panel-group" id="accordion" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingOne">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordion" href="#collapseOne" aria-expanded="true" aria-controls="collapseOne">
          CLI Steps
        </a>
      </div>
    </div>
    <div id="collapseOne" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingOne">
      <div class="panel-body">

<blockquote>
<i class="fa fa-terminal"></i> Goto the terminal and try the following:
</blockquote>
{% highlight csh %}
$ oc scale --replicas=4 dc/dc-metro-map
{% endhighlight %}

<blockquote>
<i class="fa fa-terminal"></i> Check out the new pods:
</blockquote>
{% highlight csh %}
$ oc get pods
{% endhighlight %}

Notice that you now have 4 unique pods availble to inspect.  If you want go ahead and inspect them you can see that each have their own IP address and logs (oc describe).

      </div>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingTwo">
      <div class="panel-title">
        <a class="collapsed" role="button" data-toggle="collapse" data-parent="#accordion" href="#collapseTwo" aria-expanded="false" aria-controls="collapseTwo">
          Web Console Steps
        </a>
      </div>
    </div>
    <div id="collapseTwo" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingTwo">
      <div class="panel-body">

<blockquote>
Click "Overview"
</blockquote>
<blockquote>
In the deployment, click the up arrow 3 times.
</blockquote>
The deployment should indicate that it is scaling to 4 pods, and eventually you will have 4 running pods.  Keep in mind that each pod has it's own container which is an identical deployment of the webapp.  OpenShift is now (by default) round robin load-balancing traffic to each pod.
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-replicationrecovery-4pods.png" width="500"/></p>

<blockquote>
Hover over the pod counter (the circle) and click
</blockquote>
Notice that you now have 4 unique webapp pods available to inspect.  If you want go ahead and inspect them you can see that each have their own IP address and logs.
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-replicationrecovery-4podslist.png" width="500"/></p>

      </div>
    </div>
  </div>
</div>

So you've told OpenShift that you'd like to maintain 4 running, load-balanced, instances of our web app.

### Recovery
OK, now that we have a slightly more interesting desired replication state, we can test a service outages scenario. In this scenario, the dc-metro-map replication controller will ensure that other pods are created to replace those that become unhealthy.  Let's force inflict an issue and see how OpenShift reponds.

<div class="panel-group" id="accordionB" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingBOne">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordionB" href="#collapseBOne" aria-expanded="true" aria-controls="collapseBOne">
          CLI Steps
        </a>
      </div>
    </div>
    <div id="collapseBOne" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingBOne">
      <div class="panel-body">

<blockquote>
<i class="fa fa-terminal"></i> Choose a random pod and delete it:
</blockquote>
{% highlight csh %}
$ oc get pods
$ oc delete pod/PODNAME
$ oc get pods -w
{% endhighlight %}

If you're fast enough you'll see the pod you deleted go "Terminating" and you'll also see a new pod immediately get created and from "Pending" to "Running".  If you weren't fast enough you can see that your old pod is gone and a new pod is in the list with an age of only a few seconds.

<br/><br/><i class="fa fa-info-circle"></i>  You can see the more details about your replication controller with: $ oc describe rc

      </div>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingBTwo">
      <div class="panel-title">
        <a class="collapsed" role="button" data-toggle="collapse" data-parent="#accordionB" href="#collapseBTwo" aria-expanded="false" aria-controls="collapseBTwo">
          Web Console Steps
        </a>
      </div>
    </div>
    <div id="collapseBTwo" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingBTwo">
      <div class="panel-body">

Assuming you are in the browse pods list.

<blockquote>
Click one of the running pods (not a build pod)
</blockquote>
<blockquote>
Click the "Actions" button in the top right and then select "Delete"
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-replicationrecovery-deletepod.png" width="400"/></p>

<blockquote>
Quick switch back to the Overview
</blockquote>

If you're fast enough you'll see the pod you deleted unfill a portion of the deployment circle, and then a new pod fill it back up.  You can browse the pods list again to see the old pod was deleted and a new pod with an age of "a few seconds" has been created to replace it.

<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-replicationrecovery-podrecovery.png" width="600"/></p>

      </div>
    </div>
  </div>
</div>


### Application Health
In addition to the health of your application's pods, OpenShift will watch the containers inside those pods.  Let's force inflict some issues and see how OpenShift reponds.  

<div class="panel-group" id="accordionC" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingCOne">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordionC" href="#collapseCOne" aria-expanded="true" aria-controls="collapseCOne">
          CLI Steps
        </a>
      </div>
    </div>
    <div id="collapseCOne" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingCOne">
      <div class="panel-body">

<blockquote>
<i class="fa fa-terminal"></i> Choose a running pod and shell into it:
</blockquote>
{% highlight csh %}
$ oc get pods
$ oc exec PODNAME -it /bin/bash
{% endhighlight %}

You are now executing a bash shell running in the container of the pod.  Let's kill our webapp and see what happens.
<br/><i class="fa fa-info-circle"></i> If we had multiple containers in the pod we could use "-c CONTAINER_NAME" to select the right one
<br/><br/>
<blockquote>
<i class="fa fa-terminal"></i> Choose a running pod and shell into its container:
</blockquote>
{% highlight csh %}
$ pkill -9 node
{% endhighlight %}

This will kick you out off the container with an error like "Error executing command in container"

<br/><br/>
<blockquote>
<i class="fa fa-terminal"></i> Do it again - shell in and execute the same command to kill node
</blockquote>

<blockquote>
<i class="fa fa-terminal"></i> Watch for the container restart
</blockquote>
{% highlight csh %}
$ oc get pods -w
{% endhighlight %}

If a container dies multiple times quickly, OpenShift is going to put the pod in a CrashBackOff state.  This ensures the system doesn't waste resources trying to restart containers that are continuously crashing.

      </div>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingCTwo">
      <div class="panel-title">
        <a class="collapsed" role="button" data-toggle="collapse" data-parent="#accordionC" href="#collapseCTwo" aria-expanded="false" aria-controls="collapseCTwo">
          Web Console Steps
        </a>
      </div>
    </div>
    <div id="collapseCTwo" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingCTwo">
      <div class="panel-body">

<blockquote>
Navigate to browse the pods list, and click on a running pod
</blockquote>
<blockquote>
In the tab bar for this pod, click on "Terminal"
</blockquote>
<blockquote>
Click inside the terminal view and type $ pkill -9 node
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-replicationrecovery-terminal.png" width="400"/></p>

This is going to kill the node.js web server and kick you off the container.

<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-replicationrecovery-terminalkick.png" width="400"/></p>

<blockquote>
Click the refresh button (on the terminal) and do that a couple more times
</blockquote>

<blockquote>
Go back to the pods list
</blockquote>

<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-replicationrecovery-backoff.png" width="500"/></p>

The container died multiple times so quickly that OpenShift is going to put the pod in a CrashBackOff state.  This ensures the system doesn't waste resources trying to restart containers that are continuously crashing.

      </div>
    </div>
  </div>
</div>


### Clean up
Let's scale back down to 1 replica.  If you are using the web console just click the down arrow from the Overview page.  If you are using the command line use the "oc scale" command.

## Summary
In this lab we learned about replication controllers and how they can be used to scale your applications and services.  We also tried to break a few things and saw how OpenShift responded to heal the system and keep it running.  This topic can get deeper than we've experimented with here, but getting deeper into application health and recovery is an advanced topic.  If you're interested you can read more about it in the documentation [here][1], [here][2], and [here][3].


[1]: https://docs.openshift.com/enterprise/3.1/dev_guide/application_health.html
[2]: https://docs.openshift.com/enterprise/latest/dev_guide/deployments.html#scaling
[3]: http://kubernetes.io/docs/user-guide/walkthrough/k8s201/#health-checking


# Labels

## Labels
This is a pretty simple lab, we are going to explore labels.  You can use labels to organize, group, or select API objects. 

For example, pods are "tagged" with labels, and then services use label selectors to identify the pods they proxy to. This makes it possible for services to reference groups of pods, even treating pods with potentially different docker containers as related entities.

### Labels a on pod
In a previous lab we added our web app using a S2I template.  When we did that, OpenShift labeled our objects for us.  Let's look at the labels on our running pod.

<div class="panel-group" id="accordion" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingOne">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordion" href="#collapseOne" aria-expanded="true" aria-controls="collapseOne">
          CLI Steps
        </a>
      </div>
    </div>
    <div id="collapseOne" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingOne">
      <div class="panel-body">

<blockquote>
<i class="fa fa-terminal"></i> Goto the terminal and try the following:
</blockquote>
{% highlight csh %}
$ oc get pods
$ oc describe pod/PODNAME | more
{% endhighlight %}

You can see the Labels automatically added contain the app, deployment, and deploymentconfig.  Let's add a new label to this pod.

<blockquote>
<i class="fa fa-terminal"></i> Add a label
</blockquote>
{% highlight csh %}
$ oc label pod/PODNAME testdate=4.30.2016 testedby=mylastname
{% endhighlight %}

<blockquote>
<i class="fa fa-terminal"></i> Look at the labels
</blockquote>
{% highlight csh %}
$ oc describe pod/PODNAME | more
{% endhighlight %}


<i class="fa fa-info-circle"></i> Here's a handy way to search through all objects and look at all the labels:<br/>
<i class="fa fa-terminal"></i> oc describe all | grep -i "labels:"

      </div>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingTwo">
      <div class="panel-title">
        <a class="collapsed" role="button" data-toggle="collapse" data-parent="#accordion" href="#collapseTwo" aria-expanded="false" aria-controls="collapseTwo">
          Web Console Steps
        </a>
      </div>
    </div>
    <div id="collapseTwo" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingTwo">
      <div class="panel-body">

<blockquote>
Click "Applications" and then click on "Pods"
</blockquote>
This is going to show basic details for all pods in this project (including the builders).
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-devman-allpods.png" width="500"/></p>
Next let's look at the log for the pod running our application.

<blockquote>
Click the pod for the dc metro map webapp (it shoud have a status of Running)
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-labels-poddetails.png" width="500"/></p>
Here, at the top, you can see the labels on this pod

<blockquote>
Click the "Actions" button, then click "Edit YAML" for the pod
</blockquote>
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-labels-podedit.png" width="500"/></p>
You will see all the labels under the metadata->labels section.

<blockquote>
Add a new label into the labels section
</blockquote>
Your updated label will show up in the pod's list.

      </div>
    </div>
  </div>
</div>

## Summary
That's it for this lab, now you know that all the objects in OpenShift can be labeled.  This is important because those labels can be used as part of your CI/CD process.  Advanced labs will cover using labels for Blue/Green deployments and running yours apps on specific nodes (e.g. just on SSD nodes or just on east coast nodes).  You can read more about labels [here][1] and [here][2].


[1]: https://docs.openshift.com/enterprise/latest/architecture/core_concepts/pods_and_services.html#labels
[2]: http://kubernetes.io/docs/user-guide/labels/

# CI | CD Pipelines

## Overview
In modern software projects many teams utilize the concept of continuous integration and continuous delivery (CI/CD).  By setting up a tool chain that continuously builds, tests, and stages software releases a team can ensure that their product can be reliably released at any time.  OpenShift can be an enabler in the creation and managecment of this tool chain.  In this lab we will walk through creating a simple example of a CI/CD [pipeline][1] utlizing Jenkins, all running on top of OpenShift!

### Start by creating a new project
To begin, we will create a new project. Name the new project "cicd".

<div class="panel-group" id="accordionAa" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingAaOne">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordionAa" href="#collapseAaOne" aria-expanded="true" aria-controls="collapseAaOne">
          CLI Steps
        </a>
      </div>
    </div>
    <div id="collapseAaOne" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingAaOne">
      <div class="panel-body">
        <blockquote>
        <i class="fa fa-terminal"></i> Goto the terminal and type the following:
        </blockquote>
        {% highlight csh %}
        $ oc new-project cicd
        {% endhighlight %}
      </div>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingATwo">
      <div class="panel-title">
        <a class="collapsed" role="button" data-toggle="collapse" data-parent="#accordionAa" href="#collapseAaTwo" aria-expanded="false" aria-controls="collapseAaTwo">
          Web Console Steps
        </a>
      </div>
    </div>
    <div id="collapseAaTwo" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingAaTwo">
      <div class="panel-body">

        <blockquote>Browse to original landing page, and click "New Project".</blockquote>
        <p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-cicd-new-project.png" width="100"/></p>
        <blockquote>Fill in the name of the project as "cicd" and click "Create"</blockquote>
        <p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-cicd-new-project-detail.png" width="500"/></p>
      </div>
    </div>
  </div>
</div>

### Start by installing Jenkins
First we will start by installing Jenkins to run in a pod within your workshop project.  Because this is just a workshop we use the ephemeral template to create our Jenkins sever (for a enterprise system you would probably want to use the persistent template).  Follow the steps below:


<div class="panel-group" id="accordionA" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingAOne">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordionA" href="#collapseAOne" aria-expanded="true" aria-controls="collapseAOne">
          CLI Steps
        </a>
      </div>
    </div>
    <div id="collapseAOne" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingAOne">
      <div class="panel-body">
        <blockquote>
        <i class="fa fa-terminal"></i> Goto the terminal and type the following:
        </blockquote>
        {% highlight csh %}
        $ oc new-app --template=jenkins-ephemeral -p JENKINS_PASSWORD=password
        $ oc expose svc jenkins
        $ oc policy add-role-to-user edit -z default
        {% endhighlight %}

        <blockquote>Copy hostname and paste in browser's address bar...</blockquote>

        {% highlight csh %}
        $ oc get routes | grep 'jenkins' | awk '{print $2}'
        {% endhighlight %}
      </div>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingATwo">
      <div class="panel-title">
        <a class="collapsed" role="button" data-toggle="collapse" data-parent="#accordionA" href="#collapseATwo" aria-expanded="false" aria-controls="collapseATwo">
          Web Console Steps
        </a>
      </div>
    </div>
    <div id="collapseATwo" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingATwo">
      <div class="panel-body">

        <blockquote>Click "Add to Project", click "Browse Catalog" select "jenkins-ephemeral".</blockquote>
        <p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-cicd-jenkins-ephemeral.png" width="500"/></p>
        <blockquote>Click "continue to overview", wait for it to start</blockquote>
        <p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-cicd-jenkins-start.png" width="500"/></p>
  
        <blockquote>Click the service link to open jenkins, login as admin/password</blockquote>
        <p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-cicd-jenkins-login.png" width="500"/></p>
      </div>
    </div>
  </div>
</div>

### The OpenShift pipeline plugin
Now let's make sure we have the OpenShift Pipeline [plugin][2] properly installed within Jenkins.  It will be used to define our application lifecycle and to let our Jenkins jobs perform commands on our OpenShift cluster. It is possible that the plugin is already installed in your environment, so use these steps to verify if it is installed and install it if is not.
<div class="panel-group" id="accordionB" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingBOne">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordionB" href="#collapseBOne" aria-expanded="true" aria-controls="collapseBOne">
          Install OSE Plugin Steps
        </a>
      </div>
    </div>
    <div id="collapseBOne" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingBOne">
      <div class="panel-body">
  <blockquote>Click "Manage Jenkins"</blockquote>
  <p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-cicd-manage-jenkins.png" width="100" height="100"/></p>
  
  <blockquote>Click on "Manage Plugins"</blockquote> 
  <p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-cicd-manage-plugins.png" width="500" /></p>
  
  <blockquote>Click on "Available" tab, and filter on "openshift". Find the"Openshift Pipeline Jenkins Plugin". If it is not installed, then install it.</blockquote>
  <p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-cicd-jenkins-plugin.png" width="700" /></p>
      </div>
    </div>
  </div>
</div>


You can read more about the plugin [here][3].


### Our deployments
In this example pipeline we will be building, tagging, staging and scaling a Node.js webapp.  We wrote all the code for you already, so don't worry you won't be coding in this lab.  You will just use the code and unit tests to see how CI/CD pipelines work.  And keep in mind that these principles are relevant whether your programming in Node.js, Ruby on Rails, Java, PHP or any one of today's popular programming languages.

<blockquote>Fork the project into your own GitHub account</blockquote>
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-cicd-fork.png" width="700" /></p>

<blockquote>Create a dev deployment based on the forked repo
</blockquote>
<blockquote><i class="fa fa-terminal"></i> Goto the terminal and type the following:</blockquote>
<p>
{% highlight csh %}
$ oc new-app https://github.com/YOUR_ACCOUNT/openshift-workshops.git \
   --name=dev --context-dir=dc-metro-map
$ oc expose svc/dev
{% endhighlight %}</p>

<blockquote>Create a test deployment based on a tag of the dev ImageStream
</blockquote>
<blockquote><i class="fa fa-terminal"></i> Goto the terminal and type the following:</blockquote>
<p>
{% highlight csh %}
$ oc new-app dev:readyToTest --name=test --allow-missing-imagestream-tags
$ oc expose dc/test --port 8080
$ oc expose svc/test
{% endhighlight %}</p>

#### Setup Jekins jobs to use their openshift image stream (which is off your GitHub fork)

<div class="panel-group" id="accordionC" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingCOne">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordionC" href="#collapseCOne" aria-expanded="true" aria-controls="collapseCOne">
          Steps to Create Jenkins Job
        </a>
      </div>
    </div>
    <div id="collapseCOne" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingCOne">
      <div class="panel-body">
<blockquote>click "New Item"</blockquote>
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-cicd-new-item.png" width="500" /></p>
<blockquote>call it yourname-ci-devel, select freestyle, click OK</blockquote>
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-cicd-name-job.png" width="500" /></p>
<blockquote>Click add build step and choose "Tag OpenShift Image". Enter in all the info, tag as "readyToTest"</blockquote>
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-cicd-new-tag.png" width="700" /></p>

<blockquote>In the "Post-build actions" subsection click "Add post-build action" and select "Build other projects". Type in "yourname-ci-deploytotest"</blockquote>
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-cicd-build-other-project.png" width="500" /></p>
<blockquote>Click "Save", don't worry about the error here, we are about to build that Jenkins job.</blockquote>

<p>
<b>Notes:</b> You will not need the URL of the OpenShift api endpoint or the Authorization Token<br/>
to get this to work
</p>
      </div>
    </div>
  </div>
</div>
</div>

#### Connecting the pipeline for dev->test
<div class="panel-group" id="accordionD" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingDOne">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordionD" href="#collapseDOne" aria-expanded="true" aria-controls="collapseDOne">
          Steps to Connect Pipeline
        </a>
      </div>
    </div>
    <div id="collapseDOne" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingDOne">
      <div class="panel-body">
<blockquote>Click "Back to dashboard"</blockquote>
<blockquote>Click "New Item"</blockquote>
<blockquote>Call it "yourname-ci-deploytotest", select "freestyle", click "OK"</blockquote>
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-cicd-deploy-to-test.png" width="500" /></p>
<blockquote>Click add build step and choose "Execute shell"</blockquote>
<p><img src="{{ site.baseurl }}/www/3.1/default/screenshots/ose-lab-cicd-add-exec.png" width="200" /></p>
Additional steps could go here. For now let's just add some bash to the text area:
{% highlight csh %}
echo "inside my jenkins job"
{% endhighlight%}
<blockquote>Click add build step and choose "Trigger OpenShift Deployment".</blockquote>
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-cicd-trigger-deployment.png" width="700" /></p>
<blockquote>Click add build step and choose "Scale OpenShift Deployment".</blockquote>
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-cicd-scale-deployment.png" width="700" /></p>
<blockquote>Click "Save".</blockquote>
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-cicd-save.png" width="200" /></p>
      </div>
    </div>
  </div>
</div>

### Watch me release!

At this point you should see the following scenario play out:

  * Inside of Jenkins, you will click the dev pipline that was created we created. On the left-hand side you will see an option to <img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-cicd-build-now.png" width="100" />. When you click this, the first job will begin to run. 

  * This first job will use the OpenShift Pipeline plugin to create a new tag of the image called "readyToTest".

  * When this job completes, a second job will execute. This second job cause the deployment to initiate of our test application and then scale the test application to 2 pods.

  * You can see the history of this new tag by browsing to initiate two jobs in the pipeline with the final step being the new tag of "readyToTest". The new tag can then be used for automatic or manual builds of the new test application. You can view the status of the new tag in OpenShift by browsing to Builds -> Images -> your image stream

  <p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-cicd-image-stream-view.png" width="700" /></p> 


## Summary
Coming soon...  Read more about usage of [Jenkins on OpenShift here][4].  Read more about the concepts behind [pipelines in Jenkins here][1].


[1]: https://jenkins.io/doc/pipeline/
[2]: https://wiki.jenkins-ci.org/display/JENKINS/OpenShift+Pipeline+Plugin
[3]: https://github.com/openshift/jenkins-plugin/
[4]: https://docs.openshift.com/enterprise/latest/using_images/other_images/jenkins.html
[5]: https://en.wikipedia.org/wiki/Continuous_delivery
[6]: https://github.com/openshift/origin/blob/master/examples/jenkins/README.md
[7]: https://docs.openshift.com/enterprise/latest/creating_images/s2i_testing.html#creating-images-s2i-testing

# Blue | Green Deployment

## Blue/Green deployments
When implementing continuous delivery for your software one very useful technique is called Blue/Green deployments.  It addresses the desire to minimize downtime during the release of a new version of an application to production.  Essentially, it involves running two production versions of your app side-by-side and then switching the routing from the last stable version to the new version once it is verified.  Using OpenShift, this can be very seamless because using containers we can easily and rapidly deploy a duplicate infrastructure to support alternate versions and modify routes as a service.  In this lab, we will walk through a simple Blue/Green workflow with an simple web application on OpenShift.

### Before starting
Before we get started with the Blue/Green deployment lab, let us clean up some of the projects from the previous lab. 
{% highlight csh %}
$ oc delete all -l app=jenkins-ephemeral
$ oc delete all -l app=dev
$ oc delete all -l app=test
{% endhighlight %}

### Let's deploy an application
To demonstrate Blue/Green deployments, we'll use a simple application that renders a colored box as an example. Using your GitHub account, please fork the following [project][1].

You should be comfortable deploying an app at this point, but here are the steps anyway:

> <i class="fa fa-terminal"></i> Goto the terminal and type these commands:

{% highlight csh %}
$ oc new-app --name=green [your-project-repo-url] --context-dir=dc-metro-map
$ oc expose service green
{% endhighlight %}

Note that we exposed this application using a route named "green". Wait for the application to become available, then navigate to your application and validate it deployed correctly.

### Release a new version of our app and test it in the same environment
What we'll do next is create a new version of the application called "blue". The quickest way to make a change to the code is directly in the GitHub web interface. In GitHub, edit the dc-metro-map/views/dcmetro.jade file in your repo. 

<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-bluegreen-editgithub.png" width="500"/></p>

We can change the text labels indicated by name of a color. If you want to change the label for the "Red Line", change line 22 from "Red Line" to  "Silver Line". These changes will be easily viewable on the main screen of the application. 

Use the same commands to deploy this new version of the app, but this time name the service "blue". No need to expose a new route -- we'll instead switch the "green" route to point to the "blue" service once we've verified it.

> <i class="fa fa-terminal"></i> Goto the terminal and type these commands:

{% highlight csh %}
$ oc new-app --name=blue [your-project-repo-url] --context-dir=dc-metro-map
{% endhighlight %}

Wait for the "blue" application to become avialable before proceeding.


### Switch from Green to Blue
Now that we are satisfied with our change we can do the Green/Blue switch.  With OpenShift services and routes, this is super simple.  Follow the steps below to make the switch:

<div class="panel-group" id="accordionA" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingAOne">
      <div class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordionA" href="#collapseAOne" aria-expanded="true" aria-controls="collapseAOne">
          CLI Steps
        </a>
      </div>
    </div>
    <div id="collapseAOne" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingAOne">
      <div class="panel-body">
      
<blockquote>
<i class="fa fa-terminal"></i> Goto the terminal and type the following:
</blockquote>
{% highlight csh %}
$ oc edit route green
{% endhighlight %}

This will bring up the Route configuration yaml. Edit the element spec: to: name and change it's value from "green" to "blue".

      </div>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingATwo">
      <div class="panel-title">
        <a class="collapsed" role="button" data-toggle="collapse" data-parent="#accordionA" href="#collapseATwo" aria-expanded="false" aria-controls="collapseATwo">
          Web Console Steps
        </a>
      </div>
    </div>
    <div id="collapseATwo" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingATwo">
      <div class="panel-body">

Navigate to the Routes view from the left-hand menu:
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-bluegreen-navtoroutes.png" width="500"/></p>

In your Routes overview, click on the "green" route:
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-bluegreen-routesoverview.png" width="500"/></p>

In the Route detail page, click on Actions > Edit:
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-bluegreen-routedetail.png" width="300"/></p>

Edit the Route: select the name dropdown and change the value from "green" to "blue":
<p><img src="{{ site.baseurl }}/www/3.3/default/screenshots/ose-lab-bluegreen-edit.png" width="500"/></p>
      
      </div>
    </div>
  </div>
</div>

<!--### Good work, let's clean this up
> <i class="fa fa-terminal"></i> Let's clean up all this to get ready for the next lab:

{% highlight csh %}
$ oc delete all -l subproject=dc-busses
{% endhighlight %}
-->

## Summary
Pretty easy, right?

If you want to read more about Blue/Green check out [this post][2] with a longer description as well as links to additional resources.

[1]: https://github.com/dudash/openshift-workshops
[2]: http://martinfowler.com/bliki/BlueGreenDeployment.html

# Workshop is Done

## That's it!
Hopefully, these labs provided you some idea of how to perform common tasks within the OpenShift environment.  And hopefully, you have a deeper understanding of how containers and container orchestration works.  Please feel free to continue to "kick the tires" in the demo environment we've setup and explore both the web console and the oc command line client.

## Get even deeper
While you're here, we can help you install the [Container Development Kit (CDK)][3] on your laptop.  It requires a couple large downloads, but the install process is reallly easy.  You can find the [install and getting started guides here][4].

Here are some good resources to continue learning OpenShift:
* [OpenShift Architecture][1]
* [OpenShift Developer's Guide][2]

[1]: https://docs.openshift.com/container-platform/3.3/architecture/core_concepts/index.html
[2]: https://docs.openshift.com/container-platform/3.3/dev_guide/index.html
[3]: http://developers.redhat.com/products/cdk/overview/
[4]: http://developers.redhat.com/products/cdk/docs-and-apis/







