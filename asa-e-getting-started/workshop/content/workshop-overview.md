# Getting Started with Azure Spring Apps Enterprise

Welcome to the workshop to help you get started with Azure Spring Apps Enterprise (ASA-E). The code and examples in this workshop are kept intentionally simple to help you focus on the new concepts and patterns of working with ASA-E. The goal of the workshop is to get you familiar enough with ASA-E that you know where and how to start digging in to do the more interesting work.  

## What is Azure Spring Apps Enterprise

Azure Spring Apps Enterprise is an Azure hosted solution making the deployment and management Spring based applications easier throughout their lifecycle. It comes from a collaboration between VMware and Microsoft to ease the on-ramp to bringing the power of the ecosystem to the depth of the Spring ecosystem. It includes much of the [Tanzu Application Platform](https://tanzu.vmware.com/application-platform) in a SaaS environment. This workshop will help you understand how to bring your Spring applications to ASA-E. 

## Who is this learning path for?

The primary intended student for this learning path is someone who understands the very basics of Java code and some general cloud, especially large cloud hosting provider, concepts. If you are an advanced Java user or someone who works with Azure, but not ASA-E quite a bit, there is still good information in here for you. Just expect to skip or move through some content more quickly.  


## VMware Tanzu capabilities present in Azure Spring Apps Enteprise

In addition to the base functionality that comes with Azure Spring Apps, using the Enterprise edition provides quite a bit more functionality. An instance of an Azure Spring App Enterprise will have the following capabilities available:


* [Tanzu Build Service](https://tanzu.vmware.com/build-service) - Executes reproducible container builds and keeps images up-to-date using kpack, with a supported [Cloud Native Buildpacks](https://buildpacks.io/) Platform.
* [Service Registry](https://docs.vmware.com/en/Spring-Cloud-Services-for-VMware-Tanzu/3.1/spring-cloud-services/GUID-service-registry-index.html) - Provides a highly available registry for your services to dynamically discover and call other services.
* [Application Configuration Service](https://learn.microsoft.com/en-us/azure/spring-apps/how-to-enterprise-application-configuration-service?tabs=Portal) - A central place to manage external properties for applications across all environments with Git integration.
* [VMware Spring Cloud Gateway](https://learn.microsoft.com/en-us/azure/spring-apps/how-to-use-enterprise-spring-cloud-gateway?tabs=Portal) - pring Cloud Gateway handles cross-cutting concerns for API development teams, such as single sign-on (SSO), access control, rate-limiting, resiliency, security, and more.
* [API Portal](https://learn.microsoft.com/en-us/azure/spring-apps/how-to-use-enterprise-api-portal?tabs=Portal) - API portal supports viewing API definitions from Spring Cloud Gateway for VMware Tanzu® and testing of specific API routes from the browser.


Let's now move on to an introduction to some Azure concepts.