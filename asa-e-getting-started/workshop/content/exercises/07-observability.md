= Logs and Monitoring

Logging and monitoring are key activities you do while developing an app. ASA-E tools provide valuable information as you iterate on your app. In this class we are only going to show the default Azure tools but with Enterprise you can also use [https://docs.vmware.com/en/Application-Live-View-for-VMware-Tanzu/1.1/docs/GUID-index.html](App Live View). All that is required to get more Spring specific metrics is adding [https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html](Spring Actuator) to your project and then turn it on in your application. Demonstrating how to use this tooling is beyond the scope of this class, but we thought it was important for you to know that feature is available to you.   

We used the CLI for most of the sections before this, but logging and monitoring are activities best done using the web interface.  

== Logging

Streaming the logs from your application is quite easy. Assuming you have set the default region, resource group, and service names on the following page, you just need 

```execute
az spring app logs --name simpleapp --follow
```

You should see logs from the application follow on your screen.

if you would like to see STDOUT showing up in the logs, just add a "System.out.println("xxx")" inside the sayHello method. Re-package and deploy your changes. Deploying your changes will force a break from the logs follow commands. 

Once the app is deployed, restart the follow command and you should see your string appear every time you refresh or load the web page.

If no actions happen on the server, the command will add output to let you know it is still following the logs. You will see:

```shell
 No log from server
```

Given several minutes of inactivity this command will disconnect from the log stream. 

If you want to stop following the logs, just hit CTRL+C to return the shell.

Please note that this command only displays a certain number of lines of pre-connection output and then output moving forward. In our example, we see from the beginning of Spring Boot application being started until the most recent line in the log file. This happens to be all the log output that is available. But, your log had more lines than visible, to see more lines you can add a _-- lines_ flag followed by an integer < 1000. Adding this flag will display the number of lines before the most recent log statement or until the beginning of the file, whichever comes first. 

Using Azure application logging is more full-featured method to explore your log files. 

If it is not already open, please go ahead and open the Azure Portal(https://portal.amazon.com) in another tab. Since we auth'ed to enable the CLI, the browser should still have your login information cached. 

We need to navigate to our application to explore logging and monitoring. In the middle of the page, there should be a list of all the resources you have in Azure. One line will contain an Azure Spring Apps icon and the name of the ASA-E service instance you created.

![](images/logging-asa-service-list.jpg)

If you don't see your ASA-E service listed there then click on Azure Spring Apps icon along the top with the other services. Once you click this it should bring you to a list which containers your instance of the service.

![](images/logging-asa-service-bar.jpg)

You should now be looking at the overview of your service. We are not going to explore that today and instead just stay focused on monitoring and logging. On the left nav bar you should a category called monitoring. These views will contain the tools we are about to work with. Let's start with logging by clicking on the Logs item in the list.

![](images/logging-left-nav.jpg)

STOPPED HERE. 

== aliases: []

What capabilities does Azure provide that can help in observing your application.

* _Azure Monitor_ has:
* _Monitor logs_. You need to create a workspace so that then you can forward the logs to it and then go there to analyze logs of your app.
* _Log analytics_. This is where you can get all your logs in an analytical way.
* _Application insights_. This gives you traceability of how your application is being used. Application Performance Monitoring.