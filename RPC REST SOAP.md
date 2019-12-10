https://www.smashingmagazine.com/2016/09/understanding-rest-and-rpc-for-http-apis/   

### Rest 
* REST stands for “representational state transfer,”
* REST is all about a client-server relationship, where server-side data are made available through representations of data in simple formats, often JSON
* These representations for resources, or collections of resources, are then potentially modifiable, with actions
* The actions are made discoverable via a method known as hypermedia
* Hypermedia is fundamental to REST, and is essentially just the concept of providing links to other resources.
**REST must be stateless:** not persisting sessions between requests.   
**Responses should declare cacheablility:** helps your API scale if clients respect the rules.   
**REST focuses on uniformity:** if you’re using HTTP you should utilize HTTP features whenever possible, instead of inventing conventions.   


### XML-RPC 
XML-RPC was problematic, because ensuring data types of XML payloads is tough. In XML, a lot of things are just strings, so you need to layer meta data on top in order to describe things such as which fields correspond to which data types. This became part of the basis for SOAP (Simple Object Access Protocol). 

The “RPC” part stands for “remote procedure call,” and it’s essentially the same as calling a function.

Take this example RPC call:

```
POST /sayHello HTTP/1.1
HOST: api.example.com
Content-Type: application/json

{"name": "Racey McRacerson"}
```  

The idea is the same. An API is built by defining public methods; then, the methods are called with arguments. RPC is just a bunch of functions, but in the context of an HTTP API, that entails putting the method in the URL and the arguments in the query string or body. SOAP can be incredibly verbose for accessing similar-but-different data, like reporting. If you search “SOAP example” on Google, you’ll find an example from Google that demonstrates a method named getAdUnitsByStatement, which looks like this:
```
<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope
        xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
        xmlns:xsd="http://www.w3.org/2001/XMLSchema"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <soapenv:Header>
    <ns1:RequestHeader
         soapenv:actor="http://schemas.xmlsoap.org/soap/actor/next"
         soapenv:mustUnderstand="0"
         xmlns:ns1="https://www.google.com/apis/ads/publisher/v201605">
      <ns1:networkCode>123456</ns1:networkCode>
      <ns1:applicationName>DfpApi-Java-2.1.0-dfp_test</ns1:applicationName>
    </ns1:RequestHeader>
  </soapenv:Header>
  <soapenv:Body>
    <getAdUnitsByStatement xmlns="https://www.google.com/apis/ads/publisher/v201605">
      <filterStatement>
        <query>WHERE parentId IS NULL LIMIT 500</query>
      </filterStatement>
    </getAdUnitsByStatement>
  </soapenv:Body>
</soapenv:Envelope>
```    
This is a huge payload, all there simply to wrap this argument:   
  
```

<query>WHERE parentId IS NULL LIMIT 500</query>
```    
In JavaScript, that would look like this:

``` 

/* Signature */
function getAdUnitsByStatement(filterStatement) {
  // ...
};

/* Usage */
getAdUnitsByStatement('WHERE parentId IS NULL LIMIT 500');
```   

In a simpler JSON API, it might look more like this:
```
POST /getAdUnitsByStatement HTTP/1.1
HOST: api.example.com
Content-Type: application/json

{"filter": "WHERE parentId IS NULL LIMIT 500"}
```   
Even though this payload is much easier, we still need to have different methods for getAdUnitsByStatement and getAdUnitsBySomethingElse. REST very quickly starts to look “better” when you look at examples like this, because it allows generic endpoints to be combined with query string items (for example, GET /ads?statement={foo} or GET /ads?something={bar}). You can combine query string items to get GET /ads?statement={foo}&limit=500, soon getting rid of that strange SQL-style syntax being sent as an argument.

One simple rule of thumb is this:

If an API is mostly actions, maybe it should be RPC.
If an API is mostly CRUD and is manipulating related data, maybe it should be REST.
