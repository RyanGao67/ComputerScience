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
