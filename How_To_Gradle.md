### This is a sample build.gradle
```
plugins {
    id 'java'
}

group 'com.makemake'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    testCompile group: 'junit', name: 'junit', version: '4.12'
    compile 'com.google.code.gson:gson:2.8.0'
}

```

#### Build a project.   
```
./gradlew build    
```


#### show the project object properties
```
./gradlew properties
```


### show the tasks object
```
./gradlew tasks
```


### show all the dependencies
```
 ./gradlew dependencies

```


```
(base) ➜  gradleproject0826 ./gradlew dependencies --configuration compile

> Task :dependencies

------------------------------------------------------------
Root project
------------------------------------------------------------

compile - Dependencies for source set 'main' (deprecated, use 'implementation' instead). (n)
\--- com.google.code.gson:gson:2.8.0 (n)

(n) - Not resolved (configuration is not meant to be resolved)

A web-based, searchable dependency report is available by adding the --scan option.

Deprecated Gradle features were used in this build, making it incompatible with Gradle 7.0.
Use '--warning-mode all' to show the individual deprecation warnings.
See https://docs.gradle.org/6.3/userguide/command_line_interface.html#sec:command_line_warnings

BUILD SUCCESSFUL in 462ms
1 actionable task: 1 executed

```
