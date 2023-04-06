# Creating a local Spring Application

While usually you might go to https://start.spring.io to create new application, we have already added the application code to this coding environment. The application is only using Spring Boot and Spring Web functionality.

## Editing the code

**WARNING**: This code is only for ease of teaching and NOT a pattern you should use in real life.

The top directory for the code is _./demo_

```execute-2
cd demo
```

and it has a typical Maven file layout.

For the code it will be ugly, but also simple, allowing us to focus on ASA-E rather than the code.

We need to to edit the file _demo/src/main/java/com/example/demo/DemoApplication.java_

```editor:open-file
file: ~/exercises/demo/src/main/java/com/example/demo/DemoApplication.java
```

Make the file look like this and save your changes.

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
//IMPORTANT these need to be added
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

  // Add this whole method which defines what to do when the user requests
  // the base URL of our website.
  @RequestMapping("/")
  public String helloSpring(){
     return "hello spring";
  }
 }
```

## Run  our application "locally"

Now we can see the awesomeness that is our new application. Click on the execution below which switches back to the terminal and runs Maven.

```execute-2
mvn spring-boot:run
```

See it here:

http://application-{{ session_namespace }}.{{ ingress_domain }}:{{ ingress_port }}

With that completed we have built our money making application (not really) that we need to move Azure Spring Apps Enterprise. Let's go ahead and send it over...
