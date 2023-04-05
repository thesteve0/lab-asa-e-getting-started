# Wrap up

And with that, we have finished the class. We started by introducing you to some concepts in Azure and Azure Spring Apps Enterprise (ASA-E). Then we covered how to set up ASA-E. From there, we made a Spring application (with bad code examples) that we could run "locally". We then moved that application into ASA-E and got a public URL right out of the box. Since you probably wanted to understand the development flow, we covered how to work with changes in your code. Finally, now that we have our awesome app up and running, we finished by showing you how to access your logs and monitor how your app is doing. 

One last time, in case you forgot, please delete the resource group from class. This will prevent you from accumulating charges for all the infrastructure we spun up today.

```execute
az group delete --name learning -l westus2
```

I hope this workshop showed how straight forward it was to go from some code on your laptop creating fully managed hosting that gives you access to the broad array of Azure services. There are other [Quick Starts](https://learn.microsoft.com/en-us/azure/spring-apps/quickstart-sample-app-acme-fitness-store-introduction) and [How-Tos](https://learn.microsoft.com/en-us/azure/spring-apps/how-to-enterprise-application-configuration-service?tabs=Portal) on the [Azure Site](https://learn.microsoft.com/en-us/azure/spring-apps/). Have fun exploring all the ways to use this new tool in your toolbet. 
