# catalog-spring-boot

This microservice can be run on it's own, but is intended to be run as part of the "coolstore" project in openshift.

Instructions for deploying the entire coolstore application will be available soon.

## Run catalog-spring-boot as a stand-alone RestAPI service running locally

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

To build and deploy the catalog service into OpenShift using the fabric8 maven plugin, run the following Maven command:

`$ mvn fabric8:deploy`

While you are waiting for the deploy command to complete, you can log into the OpenShift web consol and check the progress of your deployment, and even view the build and deployment logs, which should look very similar to the messages seen when running the service locally.

Once the service ha been deployed, you can get the url by running

`$ oc get route`

validate it is running using curl (or a web browser)

`curl http://CATALOGSERVICEURL/api/catalog 
 output
 
`[{"itemId":"329299","name":"Red Fedora","description":"Official Red Hat Fedora","price":34.99},...}]`

Original source:
[http://guides-cdk-roadshow.b9ad.pro-us-east-1.openshiftapps.com/index.html#/workshop/roadshow/module/spring-boot]()


