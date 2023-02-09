
# Deploying to Azure Spring Apps Enterprise (ASA-E)

Up until this point in the tutorial we haven't done anything really worth mentioning (unless you never ran a Spring Boot app before). But with all those
preliminaries complete we can demonstrate the benefits of moving your applications to ASA-E. Let's jump right in with the CLI.

## Logging in and creating our Resource Group

If you forgot the check if you were https://learn.microsoft.com/en-us/cli/azure/reference-index?view=azure-cli-latest#az-login[logged in] or not in an earlier step doing the rest of the workshop requires an authenticaded CLI. Remember we are using "--use-device-code" because, following good security practices, we can not wrap the login page in the Iframe we have here.

```execute
 az login --use-device-code
```

After executing this command, you should receive a prompt to go to a Microsoft website and enter a code. Clicking the link will open a new browser tab for the URL. You can also just copy the URL and open it in a completely different browser. Once you loging to Azure and enter the code your CLI will be authenticated for the rest of this class. When you finish with this workshop we destroy the machine hosting your workshop and delete any session information. Your password is never shared with us.

Now that you are logged in, the next step is to create a resource group to hold all the work we create today. Remember, resource group is similar to a namespace within your account. Having this created will make it exteremely easy for you to clean up all the Azure resources when we finish. Delete the resource group and everything inside it is deleted. No hunting around for random service or virtual IP floating around still costing you money.

This command will require you to pick a region where your services will be created. We suggest you pick one that is physically closest to you.  The https://stackoverflow.com/questions/44143981/is-there-an-api-to-list-all-azure-regions[following command] lists all the current availability zones:

```execute
az account list-locations -o table
```


The second column, Name, is the code you want to use in the create region command. Since the author is in California, the example will show West US 2.

```copy
az group create -l westus2 -n learning
```

This creates a Resource Group in westus2 with a name of playing1. You can use whatever name would you like for your resource group, just make sure to alter the rest of the commands to match your name.


## Creating our ASA-E Service

If you have never created an ASA-E service before, you need to accept the terms of service before continuing:

```execute
az provider register --namespace Microsoft.SaaS
az term accept --publisher vmware-inc --product azure-spring-cloud-vmware-tanzu-2 --plan asa-ent-hr-mtr
```

With that out of the way we can now create our Azure Spring Apps Enterprise service.


```copy
az spring create -n first-asa-service -g learning --sku Enterprise
```

Our ASA-E service will be named 'first-asa-service', we want to create in the resource group we just made, and we want to enable the enterprise tier.

NOTE: It can take 15 to 30 minutes to create this service. This command triggers the creation of a cluster of machines. Even if the animated icon stops spinning it is still working on creating the service. You will only need to run this command once and then you can add as many applications as you want to this service (within resource limits)

## Creating and deploying our simple app

### Creating the application
Now that we have our own ASA-E service spun up we can move our simple app into the service. There are two stages to getting an app into the service

1. First we create the application inside the service. An app is its own resource and can have 1 or more deployments.
+
``` execute
az spring app create -n simpleapp -s first-asa-service -g learning  --assign-endpoint true
```
+
This command creates an app named "simpleapp", in our service, in the learning resource group, and we want the system to give us a public endpoint

### Deploying the application

Now we package it up on our end and deploy it to the app we created:

```execute
az spring app deploy -n simpleapp --artifact-path target/demo-0.0.1-SNAPSHOT.jar -s first-asa-service -g learning
```

@TODO put in the BP_BUILD_JVM varieable, and we want it to use Java 17.
At the time of writing this workshop, the acceptable runtime versions are Java_8, Java_11, Java_17, and NetCore_3.1 for your

You should start to see this output: 

```shell
This command usually takes minutes to run. Add '--verbose' parameter if needed.
[1/5] Requesting for upload URL.
[2/5] Uploading package to blob.
...
```

You will start to see the system use [Paketo Buildpack](https://paketo.io/) to create the container image to deploy to the application service.  This process may take 2-5 minutes. In later examples we will show some hints to speed up the build process.

When the process is finished the CLI will give you some information about the deploy and the app should be ready to go. 

## Viewing our application

There are two different ways to get the URL for our application.

1. You can log in to the [Azure Portal](https://portal.azure.com/) and then navigate to the ASA-E service. So click on the _learning_ resource group, then click on _first-asa-service_, then, on the left navigation click on Apps, then click on _simpleapp_, and finally, on the top of the page will be the URL to click to see your application in action.
2. Or you can use the CLI with the following command:

```shell execute
az spring app show -n simpleapp -s first-asa-service -g learning -o table
```

Instead of creating an application, we are querying for the information on the application. If you leave off the _-o table_ there will be a quite a large JSON object returned which may make it difficult to find the URL.

Either shift+click or copy and paste the URL to see your application. 

Congratulations, you have deployed your first application to Azure Spring Apps Enterprise.

As you can see we needed to make 0 modifications to our code to get it running in ASA-E. This is one of the advavtages of ASA-E, run your same Spring Applications but let Azure handle the infrastructure and scaling issues. 
Let's go ahead and see how to make code changes and other helpful hints for daily work.









1. Login - because we can't open an iframe for the portal
 `az login --use-device-code`
 https://learn.microsoft.com/en-us/cli/azure/reference-index?view=azure-cli-latest#az-login

. ResourceGroup
 `az group create -l westus -n playing1`
 https://learn.microsoft.com/en-us/cli/azure/group?view=azure-cli-latest#az-group-create

. Spring App Service
 `az spring create -n the-asa-service -g playing1 --sku Enterprise  --enable-gateway --enable-api-portal`
 https://learn.microsoft.com/en-us/cli/azure/spring?view=azure-cli-latest#az-spring-create
 Same as spinning up any service in Azure - in this case we are spinning up resources that know how to host Spring, and other language runtimes.
 Enterprise also makes it possible to add frequently used technology, such as an API portal, to the resources spun up.

This is kinda cool to set the defaults so you don't have to keep entereing it.



. Create app on start.spring.io
 Keep all the defaults
 image:images/create-app-startspring.png[img.png]

. Then just add the Spring Web Dependency
 image:images/create-app-dependencies.png[img.png]

I think we can avoid all this if we just add this minimal code to the Workshop Image. Then they can avoid start.spring and we can open a page in an editor

. For the code it will be really ugly but also really simple. Make sure to emphasize this is for instructional purposes only and only to demonstrate how to work with ASA-E
[source,java]
```
//these need to be added
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

// Add the RestController Annotation
// This let's us respond at / with a string
// REMEMBER - this is not a good pattern to use for a normal application
@RestController
@SpringBootApplication
public class DemoApplication {

	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}

    // Add this whole section which defines what to do when the user requests
    // the base URL of our website.
	@RequestMapping("/")
	public String helloSpring(){
		return "hello spring";
	}

}
```

. Not sure I want them to run it locally - not sure that is important, though it is simple to do
. First create the app with nothing it in
`az spring app create -n springapp -s the-asa-service -g playing1  --assign-endpoint true --cpu 2 --memory 3`
. Now we package it up on our end and deploy it to the app we created
 `az spring app deploy -n springapp --artifact-path target/demo-0.0.1-SNAPSHOT.jar -s the-asa-service -g playing1`

If you area already familiar with Azure Spring Apps concepts and terminology feel free to skip
this module. This page is intended to help developer new to Azure understand some of the common concepts and terms

NOTE FOR AUTHORS The pages are based on this outline
https://onevmw-my.sharepoint.com/:w:/g/personal/spousty_vmware_com/EfM2l_jNwS5ErTm0H_JNpTEByE57Wd-nFMGw3TBDxbMMLw