## soafaces | the power of components ##
The goal of the soafaces project is to provide developers with an API for building back-end Tasklets and front-end Weblets that can be composed easily into modular components used to build jobs/workflows and web/GWT UIs.

**What can you build with soafaces?**
  * Backend modular Java Tasklet components
  * Create chain of Tasklets for job/workflow processing
  * Front-end Weblets for lightweight web UIs (and as UI customizers/editors for Tasklets)
  * Simple to complex GWT mashup apps that use web services
  * GWT API to invoke web services from any GWT client

Download latest **soafaces v2.6.20** source/jars/samples: [DOWNLOAD v2.6.20](http://code.google.com/p/soafaces/wiki/Downloads)

Specifically, the goals of the soafaces project include the following:

  1. Create back-end tasks and services that can be scheduled and run and packaged in a job with other tasks. As part of this, a task's properties and configuration can be edited and managed using a web GUI customizer that can be used to create a chain of Tasklets and allow JavaBean properties to be passed and shared between Tasklets using simple point and click.
  1. Package your code as a component and deploy your code as a modular/pluggable and self contained component. soafaces components are packaged into a simple JAR formatted archive and easily shared, deployed, and run.
  1. A framework for building web services enabled GUI applications using modular/pluggable components. Build anything from a simple AJAX type weblet all the way to a full blown web application.
  1. No need to write GWT RPC code anymore. Use the UniversalClient API to talk with POJO services that are packaged in your application server and/or talk with Mule accessible services/endpoints all across your enterprise and internet. Your GWT application will have convenient access to messaging services (SOAP, JMS, ESB ...etc) that can return JavaBeans or JSON objects back to the GWT client. All marshaling is handled by the framework.


There are three main constructs:

  1. **Tasklet** - A modular/pluggable way of build/package/deploy back-end tasks that can be run in scheduled batch jobs with convenient web services integration.
  1. **Weblet** - A modular/pluggable way to build/package/deploy front-end web applications using GWT and web services.
  1. **UniversalClient API** - An API to use from any GWT client to make web services calls with no RPC. This API can be used with our without weblets/tasklets (you can use it in any plain old GWT client). Cool way to build GWT apps without all that RPC code and with easy access to web services.

Sounds like a lot doesn't it? Well the soafaces framework is up to the task and keeps it as simple as possible. Keep reading if you would like to hear more........

#### Building Modular and Pluggable Server-Side Tasks and Jobs ####
You can use soafaces to build jobs and tasks that can be run and be scheduled in a workflow within a job processing container. The essential building block is called the Tasklet. Jobs (composed of one or more Tasklets) can be scheduled and run on a timer or executed on demand (this is up to the container to provide these services). A soafaces Tasklet can execute any arbitrary server-side java code. The input properties of a Tasklet can, optionally, be edited at design-time using a GWT customizer GUI (called a Weblet) in order to tailor the behavior of the Tasklet when it runs. So you are free to give your Tasklets full-blown GUI interfaces to customize them anyway you chose. You can also provide a Tasklet with a runtime GUI (called the RuntimeViewer) that will allow you to monitor the runtime progress and state of the Tasklet run. The Weblet and RuntimeViewer GUIs are optional, so you can just write server-side code (with no GUI) and package it up in a Bundle archive file to run in a job. Note, a Job can be composed of multiple Bundles (Taskelt and it corresponding Weblet/RuntimeViewer).

#### Building Modular and Pluggable GUI Applications ####
A Weblet is the essentially building block for building modular component based applications. It is simply a GWT client that adheres to the soafaces API for building a GUI component (called a Bundle) and provides for an easy way to deploy big or small GWT GUI applications into a container. A Weblet is a modular GWT client that you can pick up and deploy in any soafaces container. Using the power of GWT and SOA, you can build mashups that pull and combine data from different sources across your intranet or internet, allowing developers the freedom to focus on what they do best - building applications and not dealing with implementing server-side code and marshaling data between client and server.

#### Use soafaces with GWT Clients ####
With soafaces you can build and deploy web applications that are powered with web services and GWT. You can build your web GUI using the GWT (Google Web Tookit) framework and deploy your GWT applications on any standard j2ee servlet engine (like Tomcat). With soafaces, web developers can focus on building applications that can easily tap into web services hosted across the network. soafaces provides a MuleClient like API called UniversalClient. For those not familiar, the MuleClient interface is a universal messaging client available from the [Mule Project](http://mulesource.org) that can invoke services endpoints such as JMS, Web Services, SMTP, SFTP, ...etc and send and receive data. UniversalClient is an extension of the MuleClient API for AJAX/GWT applications with even more extended capabilities. UniversalClient gives a GWT client the ability to access ESB services exposed by Mule. You can also expose our own POJO services using UniversalClient and make them available from your servlet engine (WAR webapp) directly. So you can message with remote Mule services or develop your own services and access them all from the UniversalClient API.

To develop your own services, you create "local" services that you deploy in our servlet engine, and access these services using the same UniversalClient API. In fact the UniversalClient API functions as a universal messaging interface for GWT clients. It automatically marshals data in JSON and/or POJOs for you with no GWT RPC programming. The UniversalClient API builds on and extends the Mule endpoint notation, allowing you to access local POJO services within your web application (servlet engine). With this UniversalClient API you can mix and match resources and services all across the network to build GUI applications with a clear separation between the services/implementation and the GUI presentation.

### Where/How to use the soafaces Framework? ###
With soafaces you have several ways you can use the framework. You can use the framework for building modular Bundle jobs/tasks, modular Bundle GWT weblets, or as a web services messaging API in your existing GWT clients code to extend your GWT client to the open web services universe. Here is a break down of the scenarios you have for using the soafaces API:

| **Usage Scenarios** | **Deploy component where?** | **Do what with it?** | **Tasklet API** | **Weblet API** | **RuntimeViewer API** | **UniversalClient API** |
|:--------------------|:----------------------------|:---------------------|:----------------|:---------------|:----------------------|:------------------------|
| Invoke web services from GWT client| _In any standard J2EE container (e.g. tomcat, jetty...etc_ | _As a messaging API for use in any GWT client to access web services_ |  |  |  | Required |
| Build GWT Weblet UI | _Deploy in [JobServer](http://grandlogic.com) as a Weblet Application_ | _Deploy a full blown GWT application using web services with no custom RPC_ |  | Required  |  | _Optional_ |
| Build Server-side Tasklet | _Deploy in [JobServer](http://grandlogic.com) as a Tasklet/Job_ | _For scheduling batch jobs & workflow processing_ | Required | _Optional_ | _Optional_ | _Optional_ |

#### Bundles as Workflow Components ####
A Bundle can be composed of a Tasklet that uses the Tasklet API to build modular server-side Java code and that can be run as part of a larger job. The job can be scheduled, for example, and run any number of ways. The Tasklet is packed in a Bundle archive and can also include a GUI customizer using the Weblet API. A Bundle Tasklet is deployed, run and managed, all within a soafaces container that manages its life-cycle. [For details how on how create and package your own Tasklet go here](CreateSoaFacesBundle.md).

#### Bundles as Application Components ####
If you want to build a modular component application (weblet), then you can package your code as a Bundle and deploy it in a soafaces container that will run and manage your Bundle Application. A Bundle Weblet uses the the Weblet API to allow you to build GWT applications that are managed and run with a soafaces container. You of course have full access to the UniversalClient API as well, to access web services. [For details how on how create and package your own Weblet application go here](CreateSoaFacesBundle.md).


#### Standalone GWT Client ####
The soafaces framework can be used as part of your existing GWT web application and provide you the ability to connect your GWT application with POJO services in your backend webapp and to access Mule/SOA services all across your network. You can use the soafaces framework with any standard GWT application to access remote web services with no RPC coding. The UniversalClient is only one small part of the soafaces framework but it is a very powerful and useful API to use directly in your GWT web applications. The UniversalClient API provides any GWT client the ability to access Mule services/endpoints and to send/return JSON and POJO objects with minimal effort - no RPC! The API also allows creating "local" services that can be deployed within the Bundle server-side and accessed as POJOs services from the UniversalClient API. So you get simple POJOs exposed as local services with remote access provided using the UniversalClient API.

Simply include the soafaces framework JARs in with your servlet code and in your GWT code and you instantly have extended your GWT application with convenient messaging capabilities. No extra deployment or anything is needed. Simply use the soafaces framework with your existing GWT application and go. To get more information on how to [get started using the UniversalClient API with you GWT application go here](StandaloneGwtClient.md).

### Underneath the Hood ###
The essential building blocks of soafaces are tasklets and weblets. A packaged soafaces component is called a soafaces bundle (or bundle). A Bundle can be made up of a weblet and/or tasklet. The tasklet provides the API for implementing and executing back-end java code and services when the Bundle is used to build a workflow job. The weblet provides the abstraction for building AJAX Web GUIs of any kind. A weblet can be a standalone web application, that interfaces with remote web services, or it can be the GUI used to customize the input properties of a tasklet.

As discussed, soafaces also provides easy access to [Mule](http://www.mulesource.org/) services and can easily interface and communicate with remote and local Mule services of all shapes and forms. soafaces leverages the [GWT](http://code.google.com/webtoolkit/) framework which serves the basis for its Web GUI API.


---


#### Tasklets - Building Jobs ####
A tasklet is a simple way to create any arbitrary server-side java code and run it a container that supports the soafaces bundle API and run it as part of a job. The input and output of a tasklet is a JavaBean. One JavaBean for input and another for output. A tasklet consumes a JavaBean as input and returns a JavaBean as output. These input and output JavaBeans can have UI customizers/viewers that can edit and render the contents of the input/output JavaBeans for user manipluation. The Weblet API is used to render the GUI for the customizers/viewers. So you can pair a tasklet with a weblet if you want to give your tasklet a graphical representation (this is optional of course).

Multiple tasklets can be composed and packaged together into back-end workflows and executed on timers and schedules for example as part of a job. The specification defines a life cycle for the execution of one or more tasklets that are chained together to execute a single job.

#### Tasklets - Property Mapping ####
Tasklets even get more interesting and powerful when you understand how the [property mapping features](TaskletInputOutputMapping.md) works between multiple tasklets that can makeup a job. Tasklets can share and communicate with each other when they run as part of a job/workflow. Individual tasklets can pass JavaBeans to each other during the execution of the workflow (a chain of tasklets). The output JavaBean of one tasklet can serve as the input to the next tasklet in the chain (or even other tasklet further down the chain). This is done through a [mapping specification](TaskletInputOutputMapping.md) to allow the end user (business analyst type person or developer) the ability to configure which properties of one particular output JavaBean get mapped to which corresponding properties of another input JavaBean. This is a very powerful features, it allows end users to customize behavior using a simple point and click GUI where the output of one tasklet can function as input to another tasklet. It allows tasklet's with no knowledge of each other to work together to perform a larger and more complex job or workflow operation. The soafaces API makes all this possible and straight forwards.

#### Weblets and Tasklets - Design-Time Customizer for your Tasklets ####
As mentioned, if you are only interested with building a RIA GUI, then you do not have to use the Tasklets API - you just focus on an use the Weblet API (`org.soafaces.bundle.client` package). Now, if you want to build back-end jobs and workflows then you should use the Tasklets API (`org.soafaces.bundle.workflow` package), and only use weblets, along with the tasklets, if you want to build GUIs to customize your tasklet's properties. Using weblets with tasklets is optional but is very powerful as it provides you with a way to allow end users to customize the inputs of tasklet. Once you have constructed your code, you then take it and package it up into a soafaces bundle (Bundle JAR file) and deploy - that's it. It is very similar to deploying a WAR file but even simpler :)

#### RuntimeViewers and Tasklets - Monitor runtime status and progress of your Tasklet ####
The RuntimeViewer API allows you to build a custom GUI interface to monitor the progress and status of a job/tasklet as it is running or see the results after it has finished running. So think of the Weblet API as the design-time GUI for your Tasklet and the RuntimeViewer API as the runtime GUI for your Tasklet.


---


#### Weblet - Building Applications ####
The Weblet is the main abstraction for building end user GUIs with soafaces. A weblet enabled application can be deployed and run from a web application server. A weblet based application can be deployed and loaded dynamically within a hosting container without requiring server restarts. The code that makes up a soafaces Bundle is bundled and packaged in a JAR called a Bundle (soafaces Bundle).

With a weblet, you have the full power of the GWT API at your disposal. The weblet interface itself is nothing more than a GWT Composite widget that can contain any GWT widget you drop into it. It functions as the top level UI panel for your application, mashup, or any other UI thing you want to build. What is especially nice about a weblet, is that you can just compile your GWT java classes into java binary classes and include them in a JAR file and the container will handle compiling your GWT java code into actual ajax/javascript. You can use virtually any standard GWT or third party GWT widget libraries and include them in your Bundle JAR and it will all get linked and compiled for you into ajax/javascript; this sort of similar to how a j2ee server complies JSP pages on the fly. Cool don't you think! So develop your components in GWT, package them up in a Bundle, and drop them into a soafaces container and go. No need to mess with GWT javascript to java compiling!


---


#### Weblets - Accessing Remote Server-Side Services ####
Weblets and generic GWT applications can access services using the GWT UniversalClient interface. This API is similar to standard MuleClient API available in native server-side java. The GWT UniversalClient API can pass and return POJOs (basic java primitives and objects that support the GWT IsSerializable interface) and/or JSON types. So using the GWT UniversalClient interface a GWT weblet can access web services and other ESB endpoints throughout your enterprise and over the internet, and do this directly from a GWT GUI.

If we stopped there that would be interesting enough. However, you can also define your own "local" services and access such services directly from the GWT UniversalClient interface as well. A local services is a POJO method that is exposed as a local service and bundled directly within the Bundle JAR. So a Bundle can contain all the client and server/services code ,if that is what you prefer, or you can mix and match local services with remote service across your network or internet.


For example access a remote web services might go something like this:
```
  //pass POJO and returns POJO
  UniversalClient.send("axis:http://my-web-services.com/?method=hellworld", "Bob", asyncPOJOHandler);
```
All endpoints supported by Mule are supported with the GWT UniversalClient interface.

Accessing a local services would look like this:
```
  //pass POJO and returns POJO
  UniversalClient.send("soafaces://com.acme.local.MyServices/getHelloWorldEndpoint", "Bob", asyncPOJOHandler);

  //pass POJO and returns JSON
  UniversalClient.send("soafaces://com.acme.local.MyServices/getHelloWorldEndpoint", "Bob", asyncJSONHandler);
```
The prefix `soafaces://` tells the UniversalClient to route the call to a POJO service in the current Bundle JAR and return the results of the method. In this example the method `com.acme.local.MyServices.getHelloWrold("Bob")` is called and a POJO or JSON result can be returned via the `asyncHandler`.

## OSGi ##
soafaces bundles sort of sound like OSGi bundles don't they? Well, the two have similar goals with regard to building modular components that can be deployed in a runtime container and discover each other....etc. soafaces takes it a little deeper, but the underlying requirements of packaging code into components and hot deployment, for example, are very much similar. The soafaces project will make every attempt to align itself with the OSGi effort where it makes sense.

## Getting Started ##
  * Review soafaces [Javadocs](http://www.grandlogic.com/downloads_content/soafaces-javadoc/index.html)
  * Download and test drive UniversalClient [sample webapp](StandaloneGwtClient.md) to see API in action
  * Download and review sample Bundle component source code from the BeanSoup sub project and write your first Bundle
  * [Download soafaces binary and source](Downloads.md)

## [News and Events](NewsEvents.md) ##

## Products Using and Supporting soafaces ##
  * [JobServer](http://grandlogic.com) - A commercial implementation of a soafaces container for managing, deploying and running Bundles. Free for non-commercial use.
  * SoaFacesRunner is the soafaces  container Reference Implementation - you can deploy and run soafaces bundles (bundles) with SOAFacesRunner in any servlet engine. This includes deploying and running weblets and tasklets.

## Available soafaces components ##
  * BeanSoup is a small project and code with many sample soafaces bundles (bundles) implemented. A Bundle is a soafaces component that can be deployed to any compliant soafaces container.

## History ##
soafaces is an evolution of the original TaskBean component API specification. The TaskBean API was a framework before its time :) You can read more about [TaskBeans here](http://taskbean.sourceforge.net/).