
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

* How to set source and target [https://maven.apache.org/plugins/maven-compiler-plugin/examples/set-compiler-source-and-target.html](https://maven.apache.org/plugins/maven-compiler-plugin/examples/set-compiler-source-and-target.html)


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



# uber jar(fat jar)

Über is the German word for above or over (it's actually cognate with the English over).

Hence, in this context, an uber-jar is an "over-jar", one level up from a simple JAR (a), defined as one that contains both your package and all its dependencies in one single JAR file. The name can be thought to come from the same stable as ultrageek, superman, hyperspace, and metadata, which all have similar meanings of "beyond the normal".

The advantage is that you can distribute your uber-jar and not care at all whether or not dependencies are installed at the destination, as your uber-jar actually has no dependencies.

All the dependencies of your own stuff within the uber-jar are also within that uber-jar. As are all dependencies of those dependencies. And so on.

(a) I probably shouldn't have to explain what a JAR is to a Java developer but I'll include it for completeness. It's a Java archive, basically a single file that typically contains a number of Java class files along with associated metadata and resources.



ubar jar is also known as fat jar i.e. jar with dependencies.
There are three common methods for constructing an uber jar:

Unshaded: Unpack all JAR files, then repack them into a single JAR. Works with Java's default class loader. Tools maven-assembly-plugin
Shaded: Same as unshaded, but rename (i.e., "shade") all packages of all dependencies. Works with Java's default class loader. Avoids some (not all) dependency version clashes. Tools maven-shade-plugin
JAR of JARs: The final JAR file contains the other JAR files embedded within. Avoids dependency version clashes. All resource files are preserved. Tools: Eclipse JAR File Exporter


# Junit test 4&5
[https://hackernoon.com/mixing-junit4-and-junit5-2da44956de8c](https://hackernoon.com/mixing-junit4-and-junit5-2da44956de8c)



# jar basic 
Viewing the Contents of a JAR File
The basic format of the command for viewing the contents of a JAR file is:

```
jar tf jar-file
```

Let's look at the options and argument used in this command:

The t option indicates that you want to view the table of contents of the JAR file.
The f option indicates that the JAR file whose contents are to be viewed is specified on the command line.
The jar-file argument is the path and name of the JAR file whose contents you want to view.
The t and f options can appear in either order, but there must not be any space between them.

This command will display the JAR file's table of contents to stdout.

You can optionally add the verbose option, v, to produce additional information about file sizes and last-modified dates in the output.

An Example
Let's use the Jar tool to list the contents of the TicTacToe.jar file we created in the previous section:

```
jar tf TicTacToe.jar
```
This command displays the contents of the JAR file to stdout:
```
META-INF/MANIFEST.MF
TicTacToe.class
audio/
audio/beep.au
audio/ding.au
audio/return.au
audio/yahoo1.au
audio/yahoo2.au
images/
images/cross.gif
images/not.gif
```
The JAR file contains the TicTacToe class file and the audio and images directory, as expected. The output also shows that the JAR file contains a default manifest file, META-INF/MANIFEST.MF, which was automatically placed in the archive by the JAR tool. For more information, see the Understanding the Default Manifest section.

All pathnames are displayed with forward slashes, regardless of the platform or operating system you're using. Paths in JAR files are always relative; you'll never see a path beginning with C:, for example.

The JAR tool will display additional information if you use the v option:

```
jar tvf TicTacToe.jar
```

For example, the verbose output for the TicTacToe JAR file would look similar to this:
```
    68 Thu Nov 01 20:00:40 PDT 2012 META-INF/MANIFEST.MF
   553 Mon Sep 24 21:57:48 PDT 2012 TicTacToe.class
  3708 Mon Sep 24 21:57:48 PDT 2012 TicTacToe.class
  9584 Mon Sep 24 21:57:48 PDT 2012 TicTacToe.java
     0 Mon Sep 24 21:57:48 PDT 2012 audio/
  4032 Mon Sep 24 21:57:48 PDT 2012 audio/beep.au
  2566 Mon Sep 24 21:57:48 PDT 2012 audio/ding.au
  6558 Mon Sep 24 21:57:48 PDT 2012 audio/return.au
  7834 Mon Sep 24 21:57:48 PDT 2012 audio/yahoo1.au
  7463 Mon Sep 24 21:57:48 PDT 2012 audio/yahoo2.au
   424 Mon Sep 24 21:57:48 PDT 2012 example1.html
     0 Mon Sep 24 21:57:48 PDT 2012 images/
   157 Mon Sep 24 21:57:48 PDT 2012 images/cross.gif
   158 Mon Sep 24 21:57:48 PDT 2012 images/not.gif
   ```

* Understanding the Default Manifest
When you create a JAR file, it automatically receives a default manifest file. There can be only one manifest file in an archive, and it always has the pathname

META-INF/MANIFEST.MF
When you create a JAR file, the default manifest file simply contains the following:

Manifest-Version: 1.0
Created-By: 1.7.0_06 (Oracle Corporation)
These lines show that a manifest's entries take the form of "header: value" pairs. The name of a header is separated from its value by a colon. The default manifest conforms to version 1.0 of the manifest specification and was created by the 1.7.0_06 version of the JDK.

The manifest can also contain information about the other files that are packaged in the archive. Exactly what file information should be recorded in the manifest depends on how you intend to use the JAR file. The default manifest makes no assumptions about what information it should record about other files.

Digest information is not included in the default manifest. To learn more about digests and signing, see the Signing and Verifying JAR Files lesson.

# Understand the characteristics of a runnable jar
* A runnable jar must have MANIFEST.MF file under META-IN folder. This folder is recognized by java platform to configure the application, Extensions, Class Loader, and Services
* MENIFEST.MF file contains the extension and package related data, so we must declare in manifest.mf file which class contains the main method of our application if our code uses some third part libs then we need to declare the paths of those libs here. 
