### How to install oracle java 11 in ubuntu 18

```sudo add-apt-repository ppa:linuxuprising/java```  
```sudo apt-get update```   
```sudo apt-get install oracle-java11-installer-local```   
```sudo apt-get install oracle-java11-set-default-local```    
   Install oracle-java11-set-default package to set java 11 as default. Remove oracle-java11-set-default package to not set java 11 as default. 

**To check java version:**   
```java -version```   
```javac -version```   

**To uninstall**     
```sudo apt-get remove oracle-java11-set-default-local```      
and   go to Software & Updates -> Other Software to remove the PPA repository    


### How to install java8 on Ubuntu    
```sudo apt install openjdk-8-jdk```   


### Managing JAVA
You have multiple java installations on one server. You can configure which version is the default for use on the command line by using the following command:  
```sudo update-alternatives --config java```  
You can do this for other java commands, such as the compiler(javac):  
```sudo update-alternatives --config javac```  

Other commands for which this command can be run include, but are not limited to : keytool, javadoc, and jarsigner. 

**Alternatives**
apt-get won't overwrite the existing java versions.
To switch between installed java versions, use the ```update-java-alternatives``` command.
List all java versions:
```update-java-alternatives --list```

Set java version as default(needs root permissions):
```sudo update-java-alternatives --set /path/to/java/version```

Where /path/to/java/version is one of those listed by the previous command(e.g. /usr/lib/jvm/java-7-openjdk-amd64)

***update-java-alternatives is a convenience tool that uses debian's alternatives system(update-alternatives) to set  a bunch of links to the specified java version(e.g. java, javac, ...).***

### Setting the java_home environment variable
Many programs written using java use the JAVA_HOME environment variable to determine the java installation location. 

To set this environment variable, first determine where java is installed. Use the update-alternatives command:
```$ sudo update-alternatives --config java```

```
There are 3 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                            Priority   Status
------------------------------------------------------------
* 0            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1101      auto mode
  1            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1101      manual mode
  2            /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java   1081      manual mode
  3            /usr/lib/jvm/java-8-oracle/jre/bin/java          1081      manual mode

Press <enter> to keep the current choice[*], or type selection number:
```

In this case the installation paths are as follows:

1.  OpenJDK 11 is located at  `/usr/lib/jvm/java-11-openjdk-amd64/bin/java.`
2.  OpenJDK 8 is located at  `/usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java`.
3.  Oracle Java 8 is located at  `/usr/lib/jvm/java-8-oracle/jre/bin/java`.

Copy the path from your preferred installation. Then open  `/etc/environment`  using  `nano`  or your favorite text editor:

```
sudo nano /etc/environment
```

At the end of this file, add the following line, making sure to replace the highlighted path with your own copied path:

/etc/environment

```
JAVA_HOME="$(jrunscript -e 'java.lang.System.out.println(java.lang.System.getProperty("java.home"));')"
```

Modifying this file will set the  `JAVA_HOME`  path for all users on your system.

Save the file and exit the editor.

Now reload this file to apply the changes to your current session:

```
source /etc/environment
```

Verify that the environment variable is set:

```
echo $JAVA_HOME
```

You’ll see the path you just set:


Other users will need to execute the command  `source /etc/environment`  or log out and log back in to apply this setting.
