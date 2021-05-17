### Exploded archive vs Archive file
An exploded archive is a tree of folder and files that respects a given structure which your application server can exploit to deploy the application. For a web application for instance, you create a war directory structure. The application server expects a WEB-INF directory containing the web.xml files which acts as a deployment descriptor.

A packaged archive is a zip file containing the above mentioned structure. The extension of a packaged archive can vary (war, jar, car, ear) but they are all zip files that contains a given structure.

They call unzipping an archive or a zip file or a WAR file into a tree of files and folders as Exploding

### Web container
https://en.wikipedia.org/wiki/Web_container

## JakartaEE Example   
[https://github.com/RyanGao67/JavaEE_20210516](https://github.com/RyanGao67/JavaEE_20210516)
