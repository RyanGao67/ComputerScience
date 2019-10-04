### Step 1     
Check java is installed. 
```sudo update-alternatives --config java```  
```sudo update-alternatives --config javac```    
```sudo update-java-alternatives --list```   

### Step 2    
For security purposes, Tomcat should not be run under the root user. We will create a new system user and group with home directory /opt/tomcat then will run the tomcat service.  
```sudo useradd -r -m -U -d /opt/tomcat -s /bin/false tomcat```   
-r  create a system account  
-m create the user's home directory   
-U create a group with the same name as the user   
-d home directory of the new account 
-s login shell of the new account   

### Step 3     
Install Tomcat    
```wget http://www-eu.apache.org/dist/tomcat/tomcat-9/v9.0.14/bin/apache-tomcat-9.0.14.tar.gz -P /tmp```   
Go to ```tomcat.apache.org/download-90.cgi``` to check for new version   
```sudo tar xf /tmp/apache-tomcat-9*.tar.gz -C /opt/tomcat```     

To have more control over Tomcat versions and updates, we will create a symbolic link ```latest``` which will point to the Tomcat installation directory:    
```sudo ln -s /opt/tomcat/apache-tomcat-9.0.14 /opt/tomcat/latest```     
If you want to upgrade your Tomcat installation you can simply unpack the newer version and change the symlink to point to the lastest version.     
     
As mentioned in the previous section Tomcat will run under the tomcat user. This user needs to have access to the tomcat installation directory.      
```sudo chown -RH tomcat: /opt/tomcat/latest```     
chown [ OPTION ] ...  [ OWNER][:[GROUP]] FILE...    
-R operate on files and firectories recursively     
-H if a command line argument is a symbolic link to a directory, traverse it   

The scripts inside bin directory must have executable flag:
```sudo sh -c 'chmod +x /opt/tomcat/latest/bin/*.sh'```

### Step 4  
To run Tomcat as a service we will create a new unit file. 
```sudo vim /etc/systemd/system/tomcat.service```   
```
[Unit]
Description=Tomcat 9 servlet container
After=network.target

[Service]
Type=forking

User=tomcat
Group=tomcat

Environment="JAVA_HOME=/usr/lib/jvm/default-java"
Environment="JAVA_OPTS=-Djava.security.egd=file:///dev/urandom -Djava.awt.headless=true"

Environment="CATALINA_BASE=/opt/tomcat/latest"
Environment="CATALINA_HOME=/opt/tomcat/latest"
Environment="CATALINA_PID=/opt/tomcat/latest/temp/tomcat.pid"
Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"

ExecStart=/opt/tomcat/latest/bin/startup.sh
ExecStop=/opt/tomcat/latest/bin/shutdown.sh

[Install]
WantedBy=multi-user.target
```   
Modify the value of JAVA_HOME if the path to your java installation is different. 

Save and close the file and notify systemd that we created a new unit file:   
```
sudo systemctl deamon-reload
```  
Start the service status with the following command:  
```
sudo systemctl status tomcat
```  
```
* tomcat.service - Tomcat 9 servlet container
   Loaded: loaded (/etc/systemd/system/tomcat.service; disabled; vendor preset: enabled)
   Active: active (running) since Wed 2018-09-05 15:45:28 PDT; 20s ago
  Process: 1582 ExecStart=/opt/tomcat/latest/bin/startup.sh (code=exited, status=0/SUCCESS)
 Main PID: 1604 (java)
    Tasks: 47 (limit: 2319)
   CGroup: /system.slice/tomcat.service
```   
If there are no errors enable the tomcat service to be automatically started at boot time: 
```sudo systemctl enable tomcat```

### Step 5 
Adjust the firewall
If your server is protected by a firewall and you want to access Tomcat interface from the outside of your local network you need to open port 8080.     
To allow traffic on port 8080 type the following command :    
```sudo ufw allow 8080/tcp```

### Step 6   
Now that Tomcat is installed and running the next step is to create a user who will have access to the web management interface.

Tomcat users and roles are defined in tomcat-users.xml file. This file is a template with comments and examples describing how to configure the create a user or role.   

```sudo vim /opt/tomcat/latest/conf/tomcat-users.xml```    

```xml
<tomcat-users>
<!--
    Comments
-->
   <role rolename="admin-gui"/>
   <role rolename="manager-gui"/>
   <user username="admin" password="admin_password" roles="admin-gui,manager-gui"/>
</tomcat-users>

```

*By default tomcat web management interface is configured to restrict access to the manager and host manager apps only from the localhost. If you want to be able access the web interface from remote IO you will have to remove these restrictions. This may have various security implications and it is not recommended for production systems. To enable access the web interface from anywhere open the following two files and comment or remove the lines highlighed*

*For manager app*```sudo nano /opt/tomcat/latest/webapps/manager/META-INF/context.xml```    
*For host manager app*```sudo nano /opt/tomcat/latest/webapps/host-manager/META-INF/context.xml```   

```
<Context antiResourceLocking="false" privileged="true" >
<!--
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
-->
</Context>
```   

*Another option is to allow access to the Manager and Host Manager apps only from a specific IP. Instead of commenting the blocks you can simply add your IP address to the list*. Forexample:    
```
<Context antiResourceLocking="false" privileged="true" >
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1|45.45.45.45" />
</Context>

```   
```sudo systemctl restart tomcat```


### Step 6 test the tomcat

go to localhost:8080
go to localhost:8080/manager/html **You can deploy, undeploy, start, stop and reload your applictions**
go to localhost:8080/hostmanager/html		**You can create, delete and manage Tomcat virtual hosts**  
