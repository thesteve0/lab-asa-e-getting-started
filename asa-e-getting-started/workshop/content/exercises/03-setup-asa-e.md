
## Prerequisites

This lab **requires** that you have an Azure account which has access to Azure Spring Apps Enterprise.

This lab assumes you know Spring, but you are not that familiar with Azure or Azure Spring Apps Enterprise (ASA-E). Most of the important concepts used in this lab were covered in the previous exercise. If you want to go deeper you can do the [course AZ-900T00](https://docs.microsoft.com/en-us/training/courses/az-900t00), especially the last 2 modules.

It is also important to understand that, for Azure Spring Apps, the web interface is primarily read-only. You can do some very basic high-level resource creation but the majority of your work has to happen through either the command line or IDE plugin ([VSCode](https://code.visualstudio.com/docs/azure/extensions) and [Intellij](https://plugins.jetbrains.com/plugin/8053-azure-toolkit-for-intellij)). Due to this architecture, there will not be many web screenshots until we get to features such as monitoring or logging.

If you are comfortable doing all your work, in this lab environment then you are finished with the prerequisites. The next section covers what is required to do this lab outside the browser and on your own personal machine.

## Working own your own machine rather than this environment

It is possible to do all the exercises in this lab on your own machine.

There are separate how-to guides for Intellij and VScode that cover how to use the IDE plugins with Azure.

* [Intellij Azure Spring Apps](https://docs.microsoft.com/en-us/azure/spring-apps/how-to-intellij-deploy-apps) specific
* [Intellij general Azure usages](https://docs.microsoft.com/en-us/azure/developer/java/toolkit-for-intellij/)
* [VS Code Azure Spring Apps](https://code.visualstudio.com/docs/java/java-spring-apps) specific
* [VS Code general Azure usages](https://code.visualstudio.com/docs/azure/extensions)

We also assume you have a JVM &gt; version 8 and Maven already installed on your machine. They should already be set up and working in any terminal where you plan to use the Azure CLI.

Finally, we also assume you have logged in Azure from the CLI or IDE. To check with the CLI type in:

```execute
az account show
```

If you have not logged in you should be prompted to login. If you are logged in, the response should be some JSON that contains your Subscription ID, Name, and other fields. The next section gives more detailed instructions on how to login.

With those preliminaries out of the way, let's deploy your Spring Application on ASA-E.

## Configure your system and create your work area in Azure Spring Apps

In this module you are only going to create just enough application that you can learn the terminology and basic mechanics of using ASA-E for your applications. In later modules we will go into more depth.

### Logging In

The first step will be to use the CLI to login to Azure. Since Azure web console and auth follow good security practices, you have to use a device code to login.
   <br><br>
Use this command:
```execute
 az login --use-device-code
```

### Creating as resource group 

Once you have logged in, we are going to make a Resource Group to hold all our other created resources. The Resource Group will serve two purposes. First, since Azure objects need unique names, it will put all of our resources into a namespace. Second, when we are done with this module, we can delete the resource group and everything we created will be deleted.
   <br><br>
Use this command:

```copy
az group create -l westus -n learning
```

The `-l` specifies a geographic location for the resource group. Azure has [a page where](https://azure.microsoft.com/en-us/explore/global-infrastructure/geographies/#geographies) you can find the right region based on geography. To see the full list of locations along with the proper value for the -l flag, please run:

```copy
az account list-locations -o table
```

The `-n` is the name you want to give to your resource group. Here we chose "learning", but you can chose a different name if you prefer. If you do pick a different name you are going to need to change all the commands to match the name you gave your resource group.

REMEMBER when we are DONE with the workshop you should delete the resource group which delete everyting inside it.
```
az group delete --name learning -l westus2
```

### Creating the Spring Apps Enterprise Service

If you have never created an ASA-E service before, you need to accept the terms of service before continuing:

```execute
az provider register --namespace Microsoft.SaaS
az term accept --publisher vmware-inc --product azure-spring-cloud-vmware-tanzu-2 --plan asa-ent-hr-mtr
```

With that complete we are ready to make our Spring App Enterprise Service.

```copy
az spring create -n the-asa-service -g learning --sku Enterprise
```
Our ASA-E service will be named 'first-asa-service', we want to create in the resource group we just made, and we want to enable the enterprise tier.

This command will take a while to complete as Azure is spinning up a cluster for your applications.
It can take 15 to 30 minutes to create this service. This command triggers the creation of a cluster of machines. Even if the animated icon stops spinning it is still working on creating the service. You will only need to run this command once and then you can add as many applications as you want to this service (within resource limits)

<br><br>
We could have also enabled the [https://learn.microsoft.com/en-us/azure/spring-apps/how-to-use-enterprise-spring-cloud-gateway?tabs=Portal](Spring Gateway) and [https://learn.microsoft.com/en-us/azure/spring-apps/how-to-use-enterprise-api-portal?tabs=Portal](Spring API Portal) features by adding the following flags to the command above

```
--enable-gateway --enable-api-portal
```

When this is finished provisioning you now have your Azure Spring App Enterprise service up and running.

### Helpful hint

You can set some of of the defaults for the CLI so you don't have to keep re-entering it.

```
az configure --defaults group=${RESOURCE_GROUP} location=${REGION} spring=${SPRING_APPS_SERVICE}
```

If you want to remove the defaults for any of these variables you can just set the variable to an empty string. For example:

```
az configure --defaults spring=''
```


You are all set to deploy your Spring (or various [other languages](https://learn.microsoft.com/en-us/azure/spring-apps/overview#deploy-and-manage-spring-and-polyglot-applications)) applications. We are ready to create an application and deploy our simple code on our new shiny service.

