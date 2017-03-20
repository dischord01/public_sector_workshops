+++
title = "CI | CD Pipelines"
subtitle = "Jenkins plugin compatibility"
date = "2017-01-26T23:02:01-05:00"
tags = ["workshop-test-1"]
categories = ["lab", "developers", "ops"]
banner = "img/banners/banner-1.jpg"
+++



# Overview
In modern software projects many teams utilize the concept of continuous integration and continuous delivery (CI/CD).  By setting up a tool chain that continuously builds, tests, and stages software releases a team can ensure that their product can be reliably released at any time.  OpenShift can be an enabler in the creation and managecment of this tool chain.  In this lab we will walk through creating a simple example of a CI/CD [pipeline][1] utlizing Jenkins, all running on top of OpenShift!






## Step 1

{{< cli "A" >}}

oc new project cicd

{{< web "A" >}}

Browse to original landing page, and click "New Project".

{{< img src="/workshops/imported/images/englishinwhat.jpg" title="This image resides in /workshops/<*your-workshop*>/images/" >}}

{{< bq "Fill in the name of the project as 'cicd' and click 'Create' " >}}

{{< accord-end >}}



## Step 2

### Start by installing Yo Mamma.
First we will start by installing Jenkins to run in a pod within your workshop project.  Because this is just a workshop we use the ephemeral template to create our Jenkins sever (for a enterprise system you would probably want to use the persistent template).  Follow the steps below:



{{< cli "B" >}}

oc new project cicd

{{< web "B" >}}

Browse to original landing page, and click "New Project".

{{< img src="/img/Openshift-Cartridge.png" title="OpenShift Img 1" >}}

{{< bq "Fill in the name of the project as 'cicd' and click 'Create' " >}}

{{< accord-end >}}




## Step 3

### Start by installing Jenkins
First we will start by installing Jenkins to run in a pod within your workshop project.  Because this is just a workshop we use the ephemeral template to create our Jenkins sever (for a enterprise system you would probably want to use the persistent template).  Follow the steps below:

{{< cli "C" >}}

oc new project cicd

{{< web "C" >}}

Browse to original landing page, and click "New Project".

{{< img src="/img/Openshift-Cartridge.png" title="OpenShift Img 1" >}}

{{< bq "Fill in the name of the project as 'cicd' and click 'Create' " >}}

{{< accord-end >}}



# TAG: 

`rh_workshop_public_sector`

# workshop-test-1