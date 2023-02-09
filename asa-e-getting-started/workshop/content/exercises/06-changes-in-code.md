# Working with our application

Before we make any code changes, let's look at some helpful hints to make your life easier:

## Default Values
If you notice most of our commands had us specifying the resource group, service, and location. If you wre going to keep working on the same application and service then you can avoid constantly entering this information by changing the default values

```shell
az configure --defaults group=${RESOURCE_GROUP} location=${REGION} spring=${SPRING_APPS_SERVICE}
```

where we can set the default values for all 3 of these properties. In our case the command would look like (be sure to change the location to the location you chose)

```shell copy
az configure --defaults group=learning location=westus2 spring=first-asa-service 
```

If you prefer getting all your output in table format, you can also set that as the default by adding

```
output=table
```
 to the end of the command  or just running

```execute
az configure --defaults output=table
```

To clear a default, just set the property = ''. For example, if we decided we didn't want table output by defaults we would enter

```execute
az configure --defaults output=''
```

## Making a code change

Now that we have this great new app running ASA-E, let's see what's involved in making a code change
== aliases: []

Now that you have deployed your application, let's go through the process of making a simple change, since we will be making many in the following chapters to augment our application while demonstrating more of the Azure Spring Apps capabilities