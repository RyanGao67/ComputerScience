Best practice for RESTful API design is that path params are used to identify a specific resource or resources, while query parameters are used to sort/filter those resources.

Here's an example. Suppose you are implementing RESTful API endpoints for an entity called Car. You would structure your endpoints like this:

```
GET /cars
GET /cars/:id
POST /cars
PUT /cars/:id
DELETE /cars/:id
```
This way you are only using path parameters when you are specifying which resource to fetch, but this does not sort/filter the resources in any way.

Now suppose you wanted to add the capability to filter the cars by color in your GET requests. Because color is not a resource (it is a property of a resource), you could add a query parameter that does this. You would add that query parameter to your GET /cars request like this:

```
GET /cars?color=blue
```

This endpoint would be implemented so that only blue cars would be returned.

As far as syntax is concerned, your URL names should be all lowercase. If you have an entity name that is generally two words in English, you would use a hyphen to separate the words, not camel case.

Ex. /two-words


Dropwizard example:
[]()
