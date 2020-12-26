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
