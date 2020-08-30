
# How to maven

```
mvn install 
```

This command will compile, running test and package the project. Package the class files into a jar file. 



```
mvn archetype:generate -DgroupId=com.bharath -DartifactId=hellomaven -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

This command will create a project with a pom.xml

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.bharath</groupId>
  <artifactId>hellomaven</artifactId>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>hellomaven</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>


```


### Maven plugins and goals 
 a mvn plugin is a collection of one or more goals
 
 ```
 mvn archetype:generate -DgroupId=com.bharath -DartifactId=hellomaven -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

this one has a generate goal in the archetype plugin


eg. install:install    
this means install goal in the install plugin





A goal can be a specific task which we can run independently , or can be part of a bigger build


![/img/mvn1.png](/img/mvn1.png)

* How to set source and target [Setting the -source and -target of the Java Compiler](Setting the -source and -target of the Java Compiler)


### MVN coordinates
* When we build our plugins such as jar and war we'll look at the maven coordinates in the pom.xml namely the groupId and artifactID and version and packaging for the information about the project


* what represents the type of maven project we want to create  ----- archetypeartifactId

* Which maven goal can be used to create a maven project from the command line?------ archetype:generate

* Which maven plugin do we use when we create a maven project? ------- archetype


###  pluginmanagement vs plugin

* pluginManagement: is an element that is seen along side plugins. Plugin Management contains plugin elements in much the same way, except that rather than configuring plugin information for this particular project build, it is intended to configure project builds that inherit from this one. However, this only configures plugins that are actually referenced within the plugins element in the children. The children have every right to override pluginManagement definitions.


### plugin vs dependency

*  Both plugins and dependencies are Jar files.

But the difference between them is, most of the work in maven is done using plugins; whereas dependency is just a Jar file which will be added to the classpath while executing the tasks.

For example, you use a compiler-plugin to compile the java files. You can't use compiler-plugin as a dependency since that will only add the plugin to the classpath, and will not trigger any compilation. The Jar files to be added to the classpath while compiling the file, will be specified as a dependency.

Same goes with your scenario. You have to use spring-plugin to execute some spring executables [ I'm not sure what spring-plugins are used for. I'm just taking a guess here ]. But you need dependencies to execute those executables. And Junit is tagged under dependency since it is used by surefire-plugin for executing unit-tests.

So, we can say, plugin is a Jar file which executes the task, and dependency is a Jar which provides the class files to execute the task.



### TO SKip the tests:

```
-DskipTests
```

### Which scope can be used to tell maven that we do not need a dependency to be packaged in to a war that will be deployed to a container which will already have that jar/dependecy     -------->  provided



# Shading and shadowing

[https://medium.com/@akhaku/java-class-shadowing-and-shading-9439b0eacb13#:~:text=In%20Java%2C%20to%20%E2%80%9Cshade%E2%80%9D,and%20rewriting%20all%20affected%20bytecode.&text=Your%20project's%20code%20then%20uses,dependency%20on%20the%20original%20jar.](https://medium.com/@akhaku/java-class-shadowing-and-shading-9439b0eacb13#:~:text=In%20Java%2C%20to%20%E2%80%9Cshade%E2%80%9D,and%20rewriting%20all%20affected%20bytecode.&text=Your%20project's%20code%20then%20uses,dependency%20on%20the%20original%20jar.)
