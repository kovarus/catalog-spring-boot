# catalog-spring-boot

This microservice can be run on it's own, but is intended to be run as part of the "coolstore" project in openshift.

Instructions for deploying the entire coolstore application will be available soon.

## Run inventory-wildfly-swarm as a stand-alone RestAPI service running locally

Using maven, run the following command to run and test the inventory microservice locally:

`$ mvn package`

then

`$ mvn spring-boot:run`

Once you see `Started CatalogApplication in 7.327 seconds (JVM running for 8.602)` or something similar

validate it is running using curl (or a web browser)

`curl http://localhost:9000/api/catalog`
 
 output
 
`[{"itemId":"329299","name":"Red Fedora","description":"Official Red Hat Fedora","price":34.99},...}]`

terminate service `ctrl-c`

## Build and deploy catalog service on OpenShift. 

Assuming you have logged into OpenShift, make sure you are in the coolstore project:

`$ oc project coolstore`

### Deploying from local source using Maven
To build and deploy the catalog service into OpenShift using the fabric8 maven plugin, run the following Maven command:

`$ mvn fabric8:deploy`

While you are waiting for the deploy command to complete, you can log into the OpenShift web consol and check the progress of your deployment, and even view the build and deployment logs, which should look very similar to the messages seen when running the service locally.

### Deploying from github
The Java S2I image enables developers to automatically build, deploy and run java applications on demand, in OpenShift Container Platform, by simply specifying the location of their application source code or compiled java binaries. In many cases, these java applications are bootable “fat jars” that include an embedded version of an application server and other frameworks (wildfly-swarm in this instance). 

Before we start using the Java S2I image we need to tell OpenShift how to find it. This is done by creating an image stream. The image stream definition can be downloaded and used. To add the image stream to your project run the following command:

`$ oc create -f openjdk-s2i-imagestream.json`

Now you can deploy the service from github

`$ oc new-app https://github.com/bugbiteme/catalog-spring-boot.git --name catalog --image-stream=redhat-openjdk18-openshift`

A build gets created and starts building the Node.js Web UI container image. You can see the build logs using OpenShift Web Console or OpenShift CLI:

`$ oc logs -f bc/catalog`

In order to access the Web UI from outside (e.g. from a browser), it needs to get added to the load balancer. Run the following command to add the Web UI service to the built-in HAProxy load balancer in OpenShift.

~~~
$ oc expose svc/catalog
$ oc get route catalog
~~~

While you are waiting for the deploy command to complete, you can log into the OpenShift web consol and check the progress of your deployment, and even view the build and deployment logs, which should look very similar to the messages seen when running the service locally.


### Validate 
Once the service has been deployed, you can get the url by running

`$ oc get route`

validate it is running using curl (or a web browser)

`curl http://CATALOGSERVICEURL/api/catalog 
 output
 
`[{"itemId":"329299","name":"Red Fedora","description":"Official Red Hat Fedora","price":34.99},...}]`

For more information:
[http://guides-cdk-roadshow.b9ad.pro-us-east-1.openshiftapps.com/index.html#/workshop/roadshow/module/spring-boot](http://guides-cdk-roadshow.b9ad.pro-us-east-1.openshiftapps.com/index.html#/workshop/roadshow/module/spring-boot)


