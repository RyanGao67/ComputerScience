### Middleware
* Any number of functions that are invoked by the express.js routing layer before your final request handler is made

```js
var express = require('express');
var app = express();
var port = 8000;
app.use(body-parser());
app.use(cookieParser());
app.get('/', function(req, res){res.render('index')});
app.listen(port, function(){console.log(`server started on port 8000`)});
```

app.use says that the middleware is called before every route(app.get('/'))

### Example
```js
var express = require('express');
var app = express();
var port = 8000;
app.use(log);
app.get('/', log, hello);
function log(req, res, next){
  console.log(new Date(), req.method, req.url);
  next();
}
function hello(req, res){
  res.write('Hello\n'+'World');
  res.end();
  // next();
}
app.listen(port, function(){console.log('server started on port 8000')});
```
