
### How to maven

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





A goal can be a specific task which we can run indepently , or can be part of a bigger build


![/img/mvn1.png](/img/mvn1.png)
