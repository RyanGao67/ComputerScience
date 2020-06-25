In this blog I will briefly describe   
* how to setup kong as API getway.   
* How to setup keycloak as IDP(identity provider).    
* How to setup LDAP server(freeipa as the user information storage).    
* How to setup OIDC connection between Kong and keycloak.    
# Install LDAP   
# Install keycloak
Installation:
Prerequisites: Docker   
Step 1: create database in postgresDB called keycloakdb  
Step2: run docker command to install keycloak   

```
sudo docker run -d -p 8080:8080 \
-e KEYCLOAK_USER=admin \
-e KEYCLOAK_PASSWORD=admin \
-e DB_USER=keycloak \
-e DB_PASSWORD=****** \
-e DB_ADDR=10.3.9.205:5432 \
-e DB_DATABASE=keycloakdb \
-e JDBC_PARAMS='verifyServerCertificate=false&useSSL=false' \
quay.io/keycloak/keycloak:10.0.2
```
### Configurations
After successfully installing and starting Keycloak, follow these steps to connect LDAP to Keycloak and configure token settings.
* Connect to LDAP   
Go to Keycloak Admin UI.    
Go to User Federation → add provider → ldap.    
Configure settings like the following and save:    
![](img/keycloak_kong_oidc.png)  
* Go to Mappers → Create.    
To map LDAP groups to Keycloak groups, configure a mapper like the following and save:   
![](img/keycloak_kong_oidc1.png)    
* Create a client   
Go to Keycloak Admin UI.   
Go to Clients → Create.   
Configure the client settings like the following and save:   

![](img/keycloak_kong_oidc2.png) 


Go to Mappers → Create.

To map username to token claim “sub“, configure a mapper like the following and save:
![](img/keycloak_kong_oidc3.png) 

Change token settings
Go to Keycloak Admin UI.

Go to Realm Settings → Tokens.

To change token expiration time, modify the value of Access Token Lifespan.


# Setup Docker version of Kong with OIDC plugin

This part shows how to set up the docker version of Kong API Gateway with a custom plugin kong-oidc installed: . This setup includes 3 docker containers: 

* kong-with-oidc (based on Kong image)

* postgres 9.6 (Kong database)

* konga (Kong dashboard)

Server deployment:

service 
Kong proxy: http://10.3.9.241:8000   
(SSL)(http://10.3.9.241:8443)   
Kong admin API:http://10.3.9.241:8001   
(SSL)(http://10.3.9.241:8444)   
Konga dashboard: http://10.3.9.241:1337   

Steps to set up Kong   

Pull and run a postgres docker image:    

```
docker run -d --name kong-database \
                -p 5432:5432 \
                -e "POSTGRES_USER=kong" \
                -e "POSTGRES_DB=kong" \
                -e "POSTGRES_HOST_AUTH_METHOD=trust" \
                postgres:9.6
```    
Run the database migarations:     

```
docker run --rm \
    --link kong-database:kong-database \
    -e "KONG_DATABASE=postgres" \
    -e "KONG_PG_HOST=kong-database" \
    kong kong migrations bootstrap
```   
Create a Dockerfile, copy and paste the following into the file and save:   
```
FROM kong:latest

USER root
RUN luarocks install lua-resty-openidc
RUN luarocks install kong-oidc

USER kong
```    
We are using a custom Dockerfile instead of directly using the Kong image because Kong uses the non-privilege user kong, which does not not allowing writing to lua directory and therefore does not allow importing a custom plugin.

Build a image from the Dockerfile:   
```
docker build --tag kong-with-oidc .
```     
Run the kong docker image:     
```
docker run -d --name kong-with-oidc \
    --link kong-database:kong-database \
    -e "KONG_DATABASE=postgres" \
    -e "KONG_PG_HOST=kong-database" \
    -e "KONG_PROXY_ACCESS_LOG=/dev/stdout" \
    -e "KONG_ADMIN_ACCESS_LOG=/dev/stdout" \
    -e "KONG_PROXY_ERROR_LOG=/dev/stderr" \
    -e "KONG_ADMIN_ERROR_LOG=/dev/stderr" \
    -e "KONG_PLUGINS=bundled,oidc" \
    -e "KONG_ADMIN_LISTEN=0.0.0.0:8001, 0.0.0.0:8444 ssl" \
    -p 8000:8000 \
    -p 8443:8443 \
    -p 8001:8001 \
    -p 8444:8444 \
    kong-with-oidc
```    
Pull and run a konga docker image:    
```
docker run -p 1337:1337 \ 
          -e "TOKEN_SECRET=somerandomstring" \
          -e "DB_ADAPTER=postgres" \
          -e "DB_HOST=10.3.9.241" \
          -e "DB_PORT=5432" \
          -e "DB_USER=kong" \
          --name konga \
          pantsel/konga
```    
Go to Konga dashboard and set up the admin account and the Kong connection.


