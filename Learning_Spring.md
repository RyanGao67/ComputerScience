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

### Topic 4  
Read through the code about spring security
[https://github.com/RyanGao67/testKerberos/blob/master/testsecurity/src/main/java/com/makemake/testsecurity/SecurityConfiguration.java](https://github.com/RyanGao67/testKerberos/blob/master/testsecurity/src/main/java/com/makemake/testsecurity/SecurityConfiguration.java)


### Example 1:   
[https://github.com/RyanGao67/CoronavirusTracker/tree/master/cronavirus/src/main/java/com/makemake/cronavirus](https://github.com/RyanGao67/CoronavirusTracker/tree/master/cronavirus/src/main/java/com/makemake/cronavirus)   

### Example 2:   
[https://github.com/RyanGao67/SpringSecurity/tree/master/sprintJWT/src/main/java/com/makemake/sprintJWT](https://github.com/RyanGao67/SpringSecurity/tree/master/sprintJWT/src/main/java/com/makemake/sprintJWT)


### Example 3:   
how to upload file to aws with spring
[https://github.com/RyanGao67/spring-aws-image-upload/tree/master/aws-image-upload/src/main](https://github.com/RyanGao67/spring-aws-image-upload/tree/master/aws-image-upload/src/main)


### Example 4:   
[https://github.com/RyanGao67/spring-mvc/tree/master/springmvc/src/main/java/com/makemake/springmvc](https://github.com/RyanGao67/spring-mvc/tree/master/springmvc/src/main/java/com/makemake/springmvc)


### Example 5: Spring SSL
[https://github.com/RyanGao67/SpringSSL/tree/master/src/main/java/com/example/demo](https://github.com/RyanGao67/SpringSSL/tree/master/src/main/java/com/example/demo)

[https://www.thomasvitale.com/https-spring-boot-ssl-certificate/](https://www.thomasvitale.com/https-spring-boot-ssl-certificate/)



### Rest vs soap
* Rest defines an architectural approach
* Soap poses restrictions on the format of XML which is exchanged between your service provider and service consumer(SOAP defines a specific way of building web services, in soap we use XML as the request change format)

### Soap 
* for example , facebook client send XML request to todo application, todo application sends XML response to facebook client
```
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/">
  <SOAP-ENV:Header/>
  <SOAP-ENV:Body>
    <ns2:getCourseDetailsResponse xmlns:ns2="http://in28mins">
      <ns2:course>
       <ns2:id>Course1</ns2:id>
       <ns2:name>Spring</ns2:name>
       <ns2:description>10 steps</ns2:description>
      </ns2:course>
    </ns2:getCourseDetailsResponse>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

* Characteristics
  * Format
    * SOAP XML Request
    * SOAP XML response
  * Transport
    * SOAP over MQ
    * SOAP over HTTP
  * Service Definition
    * WSDL
      * EndPoint(where your service is exposed at)
      * Operations(all the operations that are exposed, eg, get code details, add a new course, delete a course)
      * Request structure(if I want to delete a course how do I send the request)
      * response structure(what response to expect)
      
      
  ### Rest
  * representational state transfer (developed by roy who developed HTTP)
  * It is developed to make the best of HTTP
  * Browser send request to server and server send back response(the request and response are in a format defined by http) 
  * Inaddition to http header and http body, it also has http methods(GET PUT POST) and HTTP status codes(400)
  * Resource is anything you want to expose to the outside through your application
  * A resource has an URI (Uniform resource identifier)
    * /user/Ranga/todos/1
    * /user/Ranga/todos
    * /user/Ranga
  * A resource has different representations
    * XML
    * HTML
    * JSON
  * The core idea is you define your resource and perform the actions on the resource using whatever facilities that are provided by HTTP. Just like we discussed earier(GET.POST)
  * Data exchange format: no restriction, JSON is popular
  * Transport : only http
  * service definition: no standard, eg, WADL/swagger..
  
  ### Rest vs SOAP
  * SOAP is a format of XML and RESTful is architecture style
  * in soap, the data exchange format is always XML , in rest there are no data exchange format, you can exhcange a XML, a json, or any other format you want to use(json is popular)
  * service definition: SOAP use WSDL(web service definition language), REST does not use a standard definition language(WADL, web application definition language, not popular, one of the popular definitions for restful web service is swagger)
  * Trasportation, SOAP does not pose any restriction(http or mq), rest use http

