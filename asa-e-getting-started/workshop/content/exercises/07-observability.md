= Logs and Monitoring

Logging and monitoring are some of the key activities you do while developing an app. Each activity provides valuable information as you iterate on your app. 

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

Next well will cover searching through our logs. 


== aliases: []

What capabilities does Azure provide that can help in observing your application.

* _Azure Monitor_ has:
* _Monitor logs_. You need to create a workspace so that then you can forward the logs to it and then go there to analyze logs of your app.
* _Log analytics_. This is where you can get all your logs in an analytical way.
* _Application insights_. This gives you traceability of how your application is being used. Application Performance Monitoring.