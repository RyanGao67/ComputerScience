### Topic 1: Building Hypermedia-Driven Restful webservice 
* Spring initializr(Dependencies: Spring Hateoas)
* Add the following to pom.xml.Because you need a JSON to send and receive information
```xml
<dependency>
<groupId>com.jayway.jsonpath</groupId>
<artifactId>json-path</artifactId>
<scope>test</scope>
</dependency>
```
* src/main/java/com/example/resthateoas/Greeting.java
```java
package com.example.resthateoas;
import org.springframework.hateoas.RepresentationModel;
import com.fasterxml.jackson.annotation.JsonCreator;
import com.fasterxml.jackson.annotation.JsonProperty;
public class Greeting extends RepresentationModel<Model>{
// JsonCreator: signals how jackson can create an instance of this pojo
// JsonProperty: Marks the field into which jackson should put this constructor argument
  private final String content;
  @JsonCreator
  public Greeting(@JsonProperty("content") String content){
    this.content = content;
  }
  public String getContent(){
    return content;
  }
}

```

* src/main/java/com/example/resthateoas/GreetingController.java

```java
package com.example.resthateoas;
import static org.springframework.hateoas.server.mvc.WebMvcLinkBuilder.*;
import org.springframework.http.HttpEntity;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;

// The Get request should return a 200 OK response with JSON in the body to represent a greeting
// Beyond that the json representation of the reosurce will be enriched with a list of hypermedia elements in a _links
// property. 
//{
//"content":"Hello world",
//"_links":{
//  "self":{"href":"http://localhost:8080/greeting?name=World"}
//}
//}



// Because @RestController annotation is present on the class, an implicit @ResponseBody
// annotation is added to the greeting method, this causes Spring MVC to render the returned 
// HttpEntity and its payload (greeting) directly to the response
@RestController
public class GreetingController{
  private static final String TEMPLATE="Hello, %s";
  @RequestMapping("/greeting")
  public HttpEntity<Greeting> greeting(@RequestParam(value="name", defaultValue="World") String name){
    Greeting greeting = new Greeting(String.format(TEMPLATE, name));
    greeting.add(linkTo(methodOn(GreetingCOntroller.class).greeting(name)).withSelfRel());
    return new ResponseEntity<>(greeting, HttpStatus.OK);
  }
}
```


Topic 2 Serving web content with Spring MVC
* Spring initializr (Spring Web, Thymeleaf, Spring Boot Devtools)

```java
// src/main/java/com/example/servingwebcontent/ServingWebContentApplication.java
package com.example.sevingwebcontent;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
@SpringBootApplication
public class ServingWebContentApplication{
  public static void main(String[] args){
    SpringApplication.run(ServingWebContentApplication.class, args);
  }
}
```

```java
// src/main/java/com/example/servingwebcontent/GreetingController.java
package com.example.servingwebcontent;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class GreetingController{
  @GetMapping("/greeting")
  public String greeting (@RequestParam(name="name", required=false, defaultValue="World") String name, Model model){
    model.addAttribute("name", name);
    return "greeting";
  }
}
```
/src/main/resources/templates/greeting.html
```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head><title>Getting Started: Serving Web Content</title><meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/></head>
<body><p th:text="'Hello, '+${name} +'!'"/></body>
</html>
```


### Topic 2
Read throught the code.   
[https://github.com/RyanGao67/SpringBasic20200531/blob/master/src/main/java/com/makemake/springbasic20200524/Springbasic20200524Application.java](https://github.com/RyanGao67/SpringBasic20200531/blob/master/src/main/java/com/makemake/springbasic20200524/Springbasic20200524Application.java)

### Topic 3
Read through the code.
[https://github.com/RyanGao67/Sprintboot0601/blob/master/src/main/java/com/makemake/springboot0601/api/PersonController.java](https://github.com/RyanGao67/Sprintboot0601/blob/master/src/main/java/com/makemake/springboot0601/api/PersonController.java)
