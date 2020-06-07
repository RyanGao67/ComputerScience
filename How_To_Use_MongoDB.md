
# How to develop a simple single web app with cloud 9
### Tools
* cloud9 
* express
* ejs
* HTML and CSS
* MongoDB

### Setting up database
First, I'll set up the database and the database I used for this project is MongoDB. 
* Install MongoDB to cloud 9 using the following command
```
$ sudo apt-get install mongodb-org -y 
```
The ```-y``` means that "Assume Yes to all queries and do not prompt".
Second, I'll need to create a folder to store mongodb data. 
```
$ mkdir data
```
Third, I need to use ```mongod``` which means "mongo daemon" that runs in the background when we use mongodn. 

```
$ echo 'mongod --bind_ip=$IP --dbpath=data --nojournal --rest "$@"' > mongod
$ chmod a+x mongod
```
--dbpath=data - Because it defaults to /var/db (which isn’t accessible)
--nojournal - Because mongodb usually pre-allocates 2 GB journal file (which exceeds Cloud9 disk space quota)
--bind_ip=$IP - Because you can’t bind to 0.0.0.0
--rest - Runs on default port 28017

### Mongodb basic
**CRUD** create, read, update, delete
* mongod
This command will start the mongo daemon.
* mongo
After the mongodaemon is started, we can use this command to use mongoDB. After running this command, a new mongo shell is ready for us to use. 
* help
In the mongo shell, running this command will show us the help menu. 
* show dbs
Show all the databases managed by mongoDB.
* use 
Creating a database is the same way as using a database. 
```
> use toronto
```
* insert
```
db.toronto.insert({name:"Ryan", age:23})
db.toronto.insert({name:"Eleanor", age:23, sex:"female"})
```
Here `toronto` is the name of collection(compare this to table in sql databases). 
```
show collections
```

* find
```
db.toronto.find()
```
```
db.toronto.find({name:"Ryan"})
```
* update
```
db.toronto.update({name:"Ryan"}, {sex:"male"})
db.toronto.update({name:"Eleanor"}, {$set:{name:"Victor", isCute:"true"}})
```
* remove
```
db.toronto.remove({sex:"male"})
```
One thing to mention is that we can't set a limit when using `remove` or `findAndModify`. Here is the way to bypass it.
```
db.collectionName.find({},{_id:1})
.limit(100)
.sort({timestamp:-1})
.toArray().
map(function(doc){return doc._id;});

db.collectionName.remove({_id:{$in: removeIdsArray}})
```
Here the find command has two parameters, the first one the query, the second is the fields we want to see for each chosen records. 


### How to connect to mongoDB with mongoose in javascript(MongoDB object modelling)

`npm install mongoose`
```
var mongoose = require("mongoose");
mongoose.connect("mongodb://localhost/blog_demo");//blog_demo is the database, if there's not such database, it will be created 
//USER - email, name
var userSchema = new mongoose.Schema({
    email: String, 
    name: String,
    posts:[postSchema]
});
var User = mongoose.model("User", userSchema);
var newUser = new User({
    email:"hermione@hogwarts.edu",
    name:"Hermione Granger"
});
newUser.posts.push({
    title:"How to bre polyjuice potion",
    content:"Goto potion class to learn it"
});
newUser.save(function(err, user){
    if(err){console.log(err);}
    else{console.log(user);}
})
//POST - title, content
var postScheme = new mongoose.Schema({
    title:String, 
    content: String
});

var postModel = mongoose.model("Post", postScheme);

var newUser = new User({
    email:"charlie@brown.edu",
    name:"Charlie Brown"
});

newUser.save(function(err, user){
    if(err){console.log(err);}
    else{console.log(user);}
})

User.findOne({name:"Hermione Granger"}, function(err, user){if(err){console.log(err)}
    else{user.posts.push({
        title:"3thing",
        content:"voldemort"
    });
        user.save(function(err, user){
            if(err)console.log(err);
            else{console.log(user);}
        })
    }
})

```
Another kind of data association using reference
```
var userSchema = new mongoose.schema(
{email:String,
    name:String,
    posts:[{
        type:mongoose.Schema.Types.ObjectId,
        ref:"Post"
    }]
}
);
var User = mongoose.model("User", userSchema);
User.create({email:"bob@gmail.com",
    name:"Bob Belcher"
});
Post.create({
    title:"how to cook the best burger",
    content:"blah blah"
}, function(err, post){
    console.log(post);
    User.findOne({email:"bob@gmail.com"}, function(err, foundUser){
    if(err)console.log(err);
    else{
        fountdUser.posts.push(post);
    }
        foundUser.save(function(err, data){
            if(err){console.log(err);}
            else{console.log(data);}
        })
    })
});
```
