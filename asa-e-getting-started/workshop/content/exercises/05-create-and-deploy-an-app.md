# Creating an App and Deploying to Azure Spring Apps Enterprise (ASA-E)

Let's jump right in with the CLI.


## Creating and deploying our simple app

### Creating the application
Now that we have our own ASA-E service spun up we can move our simple app into the service. There are two stages to getting an app into the service

First we create the application inside the service. An app is its own resource and can have 1 or more deployments.

```copy
az spring app create -n simpleapp -s the-asa-service -g learning  --assign-endpoint true
```

This command creates an app named "simpleapp", in our service, in the learning resource group, and we want the system to give us a public endpoint. The public endpoint will be an URL pointing to our application accessible outside Azure.

### Deploying the application

Now we package our simple app and and deploy it to the app we just created:

```execute
az spring app deploy -n simpleapp --artifact-path target/demo-0.0.1-SNAPSHOT.jar -s the-asa-service -g learning
```

If you wanted to deploy from source rather than from an artifact you could use --source-path instead of --artifact-path

You can set different JVM version for the build by adding a 

```shell
--build-env=BP_JVM_VERSION=11.*
```

You can see more build variables in this [how-to guide](https://learn.microsoft.com/en-us/azure/spring-apps/how-to-enterprise-deploy-polyglot-apps#deploy-a-polyglot-application).

You should start to see this output: 

```shell
This command usually takes minutes to run. Add '--verbose' parameter if needed.
[1/5] Requesting for upload URL.
[2/5] Uploading package to blob.
...
```

You will start to see the system use [Paketo Buildpack](https://paketo.io/) to create the container image to deploy to the application service.  This process may take 2-5 minutes. 

When the process is finished the CLI will give you some information about the deploy. At this point the application should be ready to receive requests. 

## Viewing our application

There are two different ways to get the URL for our application.

1. You can log in to the [Azure Portal](https://portal.azure.com/) and then navigate to the ASA-E service. To do this, click on the _learning_ resource group, then click on _first-asa-service_, then, on the left navigation click on Apps, then click on _simpleapp_, and finally, on the top of the page will be the URL to click to see your application in action.
2. Or you can use the CLI with the following command:

```shell execute
az spring app show -n simpleapp -s the-asa-service -g learning -o table
```

Instead of creating an application with this command, we are querying for the information on the application. If you leave off the _-o table_ there will be a quite a large JSON object returned which may make it difficult to find the URL.

Either _shift+click_ or copy and paste the URL to see your application. 

Congratulations, you have deployed your first application to Azure Spring Apps Enterprise.

As you can see we needed to make 0 modifications to our code to get it running in ASA-E. This is one of the advantages of ASA-E: run your same Spring Applications but let Azure handle the infrastructure and scaling issues. 

Let's go ahead and see how to make code changes to our application.
