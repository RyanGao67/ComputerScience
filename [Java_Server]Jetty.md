# Introduction to Jetty   
Jetty is an open source project providing an HTTP server, an HTTP client, and a Java servlet container. 

The project is a part of the Eclipse Foundation. 

Jetty supports additional Java technologies including: 
* SPDY
* WebSockets
* JNDI
* JAAS
* OSGI
* AJP
* JMX

# Steps
1. Download:    
* [https://www.eclipse.org/jetty/download.php](https://www.eclipse.org/jetty/download.php)

2. tar xzf ...

3. cd .... eg, `cd jetty-9.2.2`

4. 

```
$ ls -F
bin/        lib/                        modules/     resources/  start.jar
demo-base/  license-eplv10-aslv20.html  notice.html  start.d/    VERSION.txt
etc/        logs/                       README.TXT   start.ini   webapps/
```
The bin directory contains utility scripts to help run Jetty on Unix. 

The lib directory contains all Jar files necessary to run Jetty. 

The modules directory has module definitions where a module is a configuration file that includes all the libraries, dependencies, XML and template INI files for a Jetty feature. 

The resources directory contains additional resources for the classpath. 

The start.jar is used to invoke Jetty. 

The demo-base directory contains demo applications distributed with Jetty. 

The start.d directory contains INI files that contain arguments that are added to the command line. (For instance, the port number of the HTTP module on which Jetty listens.) 

The etc is a directory for Jetty XML configuration files. 

The logs is a directory for logs. 

The start.ini file contains arguments that are added during command line execution. 

Finally, the webapps directory contains web applications that run under the default configuration of Jetty. It is where we deploy our web archives.

# Start demo

cd demo-base

java -jar ../start.jar


# Shutting down Jetty
The Jetty server can be stopped by pressing Ctrl+C at the shell prompt or by killing its process.

It is possible to stop Jetty with the start.jar. Stopping this way must be enabled first.

$ java -jar start.jar STOP.PORT=8090 STOP.KEY=mypasswd 

When we start Jetty, we provide a password and a port number on which Jetty listens to stop command.

$ java -jar start.jar STOP.PORT=8090 STOP.KEY=mypasswd --stop

 
We append the --stop option to shut down the Jetty server.


# Jetty home & base

Starting with Jetty 9.1, it is possible to maintain a seperation between the binary installation of the standalone Jetty called Jetty home, and the customizations for a specific environment called Jetty base. 


* Jetty home is the location for the Jetty distribution binaries, default XML configurations, and default module definitions. 

* Jetty base is the location for configurations and customizations to the Jetty distribution.

```
$ mkdir my-base
$ cd my-base/
```

We create a my-base directory which will be our Jetty base. 

```
$ export JETTY_BASE=/home/...../my_base

```

A JETTY_BASE environment variable is created. Jetty determins the Jetty home and Jetty base locations either from environment variables or from properties

Three important items of a Jetty base are the start.d configuration directory, the start.ini configuration file and the webapps directory. 

We use the start.jar to enable necessary modules of Jetty. 

The --add-to-start option enables a module by appending lines to the ${jetty.base}/start.ini file. 

The --add-to-startd enables a module via creation of a module-specific INI file in the ${jetty.base}/start.d/ directory. 

So, 
```
java -jar $JETTY_HOME/start.jar --add-to-start=deploy
```

This will create start.ini file and add deploy module to it. Also the webapps directory is created. 


```
$ java -jar $JETTY_HOME/start.jar --add-to-startd=http

```

The http module configuration named http.ini is created in the start.d directory.

```
$ tree
.
├── start.d
│   └── http.ini
├── start.ini
└── webapps
```
2 directories, 2 files
At this moment we have this content in our Jetty base directory. Actually, we have enabled more than two modules—modules may have dependent modules and these were enabled as well. For instance, by enabling the http module we have activated the server module as well.
```
$ java -jar $JETTY_HOME/start.jar --list-modules
...
Jetty Active Module Tree:
-------------------------
 + Module: server [enabled]
   + Module: http [enabled]
   + Module: security [enabled]
   + Module: servlet [enabled]
     + Module: webapp [enabled]
       + Module: deploy [enabled]
```

To change the port
```
$ cat start.d/http.ini 
```

# To Start the app 
```
$ pwd
/home/janbodnar/prog/jetty/my-base
$ java -jar $JETTY_HOME/start.jar
```

# Create webapp
[https://github.com/RyanGao67/Learn_Jetty_20201226/tree/master/src/main/java/com/journaldev](https://github.com/RyanGao67/Learn_Jetty_20201226/tree/master/src/main/java/com/journaldev)

# Reference
Jetty:   
[http://zetcode.com/java/jetty/introduction/](http://zetcode.com/java/jetty/introduction/)

Jersey:    
[https://www.journaldev.com/498/jersey-java-tutorial#creating-jersey-eclipse-maven-project](https://www.journaldev.com/498/jersey-java-tutorial#creating-jersey-eclipse-maven-project)


[https://www.vogella.com/tutorials/REST/article.html](https://www.vogella.com/tutorials/REST/article.html)

[https://www.baeldung.com/deploy-to-jetty](https://www.baeldung.com/deploy-to-jetty)


### 3. Jetty Struction
**Context path**  Refers to the location which is relative to the server’s address and represents the name of the web application.

For example, if our web application is put under the $JETTY_HOME\webapps\myapp directory, it will be accessed by the URL http://localhost/myapp, and its context path will be /myapp.

**WAR.** Is the extension of a file that packages a web application directory hierarchy in ZIP format and is short for Web Archive. Java web applications are usually packaged as WAR files for deployment. WAR files can be created on the command line or with an IDE like Eclipse.

### 4. Deploying by Copying WAR
The easiest way to deploy a web application to Jetty server is probably by copying the WAR file into the $JETTY_HOME/webapps directory.

After copying, we can start the server by navigating to $JETTY_HOME and running the command:

java -jar start.jar   
Jetty will scan its $JETTY_HOME/webapps directory at startup for web applications to deploy.

[https://www.baeldung.com/deploy-to-jetty](https://www.baeldung.com/deploy-to-jetty)


