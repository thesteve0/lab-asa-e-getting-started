# Working with our application

Before we make any code changes, let's look at some helpful hints to make your life easier:

## Default Values - THIS ALSO APPEARS IN SETUP LESSON
If you notice most of our commands had us specifying the resource group, service, and location. If you wre going to keep working on the same application and service then you can avoid constantly entering this information by changing the default values

```shell
az configure --defaults group=${RESOURCE_GROUP} location=${REGION} spring=${SPRING_APPS_SERVICE}
```

where we can set the default values for all 3 of these properties. In our case the command would look like (be sure to change the location to the location you chose)

```shell copy
az configure --defaults group=learning location=westus2 spring=the-asa-service 
```

If you prefer getting all your output in table format, you can also set that as the default by adding

```
output=table
```
 to the end of the command or just running

```execute
az configure --defaults output=table
```

To clear a default, just set the property = ''. For example, if we decided we didn't want table output by defaults we would enter

```execute
az configure --defaults output=''
```

## Making a code change

Now that we have this great new app running ASA-E, let's see what's involved in making a code change. In keeping with the
theme of simple but ugly code, go ahead and change the text in DemoApplication.java. If you closed the page, you can reopen it using the statement below. Change the text to anything you want, as long as it is different.

```editor:open-file
file: ~/exercises/demo/src/main/java/com/example/demo/DemoApplication.java
```

Once you are finished with your changes, go back to the terminal and run the following command

```execute
mvn package
```

This tells Maven to go ahead compile the project with any changes and rebuild the Jar file. If you want to check, you can go into the _target_ directory and run `ls -a`, the timestamp on the Jar file should be different from the other files in the directory. 

With our new Jar file in hand it's time to do another deployment. If we wanted to do a [blue-green deployment](https://martinfowler.com/bliki/BlueGreenDeployment.html), we could give the deployment a name, otherwise, by default, the deployment will just update the running application.

Redeploying our code is simple as rerunning the exact same command of our first deploy. If you are used to the Linux terminal you will know you can just hit the up arrow twice and there is your command.

```execute
az spring app deploy -n simpleapp --artifact-path target/demo-0.0.1-SNAPSHOT.jar -s the-asa-service -g learning
```

Refresh your browser, and you should see the change in the text on the page. 

With our application deployed and running, let's turn now to logging and monitoring.

