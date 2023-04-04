
== Prerequisites

This lab *requires* that you have an Azure account which has access to Azure Spring Apps Enterprise.

This lab assumes you know Spring, but you are not that familiar with Azure or Azure Spring Apps Enterprise (ASA-E). Most of the important concepts used in this lab were covered in the previous exercise. If you want to go deeper you can do the https://docs.microsoft.com/en-us/training/courses/az-900t00[course AZ-900T00], especially the last 2 modules.

It is also important to understand that, for Azure Spring Apps, the web interface is primarily read-only. You can do some very basic high-level resource creation but the majority of your work has to happen through either the command line or IDE plugin (https://code.visualstudio.com/docs/azure/extensions[VSCode] and https://plugins.jetbrains.com/plugin/8053-azure-toolkit-for-intellij[Intellij]). Due to this architecture, there will not be many web screenshots until we get to features such as monitoring or logging.

If you are comfortable doing all your work, in this lab environment then you are finished with the prerequisites. The next section covers what is required to do this lab outside the browser and on your own personal machine.

== Working own your own machine rather than this environment

It is possible to do all the exercises in this lab on your own machine.

There are separate how-to guides for Intellij and VScode that cover how to use the IDE plugins with Azure.

* https://docs.microsoft.com/en-us/azure/spring-apps/how-to-intellij-deploy-apps[Intellij Azure Spring Apps] specific
* https://docs.microsoft.com/en-us/azure/developer/java/toolkit-for-intellij/[Intellij general Azure usages]
* https://code.visualstudio.com/docs/java/java-spring-apps[VS Code Azure Spring Apps] specific
* https://code.visualstudio.com/docs/azure/extensions[VS Code general Azure usages]

We also assume you have a JVM &gt; version 8 and Maven already installed on your machine. They should already be set up and working in any terminal where you plan to use the Azure CLI.

Finally, we also assume you have logged in Azure from the CLI or IDE. To check with the CLI type in:

[source,console,role=execute,copy]
----
az account show

----

If you have not logged in you should be prompted to login. If you are logged in, the response should be some JSON that contains your Subscription ID, Name, and other fields. The next section gives more detailed instructions on how to login.

With those preliminaries out of the way, let's deploy your Spring Application on ASA-E.

== Configure your system and create your work area in Azure Spring Apps

In this module you are only going to create just enough application that you can learn the terminology and basic mechanics of using ASA-E for your applications. In later modules we will go into more depth.

. The first step will be to use the CLI to login to Azure. Since Azure web console and auth follow good security practices, you have to use a device code to login.
+
Use this command:
+
[source,console,role=execute,copy]
----
 az login --use-device-code
----
. Once you have logged in, we are going to make a Resource Group to hold all our other created resources. The Resource Group will serve two purposes. First, since Azure objects need unique names, it will put all of our resources into a namespace. Second, when we are done with this module, we can delete the resource group and everything we created will be deleted.
+
Use this command:
+
[source,console,role=execute,copy]
----
az group create -l westus -n learning
----
+
The `+-l+` is specifies a geographic location for the resource group. Azure has (https://azure.microsoft.com/en-us/explore/global-infrastructure/geographies/#geographies)[a page where] you can find the right region based on geography.
+
The `+-n+` is the name you want to give to your resource group. Here we chose "learning", but you can chose a different name if you prefer. If you do pick a different name you are going to need to change all the commands to match the name you gave your resource group.

. With that complete we are ready to make our Spring App Enterprise Service.
+
```copy
az spring create -n the-asa-service -g learning --sku Enterprise
```

This command will take a while to complete as Azure is spinning up a cluster for your applications.

When this is finished provision you now have your Azure Spring App Enterprise service up and running. We could have also
enabled the Spring Gateway and Spring API Portal features by adding the following flags to the command above

```
--enable-gateway --enable-api-portal
```

You are all set to deploy your Spring (or various https://learn.microsoft.com/en-us/azure/spring-apps/overview#deploy-and-manage-spring-and-polyglot-applications[other languages]). We are ready to create an application and deploy some code on our new shiny service:

This is nice feature to set some of the defaults for the CLI so you don't have to keep re-entering it.

```
az configure --defaults \
    group=${RESOURCE_GROUP} \
    location=${REGION} \
    spring=${SPRING_APPS_SERVICE}
```
