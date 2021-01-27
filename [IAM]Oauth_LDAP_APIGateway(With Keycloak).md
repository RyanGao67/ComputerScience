### Update 20210119
https://spoore.wordpress.com/2017/02/21/keycloak-and-freeipa-intro/

Here is one example for using keycloak with Spring boot: 
[https://github.com/RyanGao67/Spring_Keycloak/tree/master/demo/src/main/java/com/example/demo](https://github.com/RyanGao67/Spring_Keycloak/tree/master/demo/src/main/java/com/example/demo)

Here is the api gateway: 
[https://github.com/RyanGao67/API_Gateway/blob/master/demo/src/main/resources/application.properties](https://github.com/RyanGao67/API_Gateway/blob/master/demo/src/main/resources/application.properties)

![./img/keycloak1.png](./img/keycloak1.png)

Authorization code grant:

 you have experienced this type of authorization flow when granting a mobile application or a Web page to access your Facebook or Google account. And this is a redirection based flow.  To use this flow, it must be capable of handling your redirects from the authorization server. And additionally, to be able to handle your redirects, the client application must be able to securely store the client secret that it will be using when exchanging the authorization code for an access token.

And if the client application cannot guarantee this, Instead, it should consider using PKCE in the hands authorization code.

* How the workflow works:
[https://docs.google.com/presentation/d/1CiAiuay5rd1KDDnYwOyu6ud9xk5ZetSQDOMp9DYUKjs/edit#slide=id.g8bb7b0e120_0_0](https://docs.google.com/presentation/d/1CiAiuay5rd1KDDnYwOyu6ud9xk5ZetSQDOMp9DYUKjs/edit#slide=id.g8bb7b0e120_0_0)


PKCE-enhanced Authorization Code:

[https://docs.google.com/presentation/d/1yJeYPMoPY2050cZkkBcBu1SL5Z88StV7O7fH5f_6X3A/edit#slide=id.g8b2fa1d558_0_2](https://docs.google.com/presentation/d/1yJeYPMoPY2050cZkkBcBu1SL5Z88StV7O7fH5f_6X3A/edit#slide=id.g8b2fa1d558_0_2)


Client Credentials: 
[https://docs.google.com/presentation/d/1KEA3i0F0bhB4me1uHfXkbmuaaFeRyxo7rG0ih-MlP68/edit#slide=id.g8bcad8744b_0_1](https://docs.google.com/presentation/d/1KEA3i0F0bhB4me1uHfXkbmuaaFeRyxo7rG0ih-MlP68/edit#slide=id.g8bcad8744b_0_1)



Refresh AccessToken:
[https://docs.google.com/presentation/d/1e0bWzKk5JxxGXlAvAyeWe1q8iYgY804Y6vZ-zCpRAKU/edit#slide=id.p](https://docs.google.com/presentation/d/1e0bWzKk5JxxGXlAvAyeWe1q8iYgY804Y6vZ-zCpRAKU/edit#slide=id.p)


### Update 20200701
* If keycloak ssl required
[https://stackoverflow.com/questions/30622599/https-required-while-logging-in-to-keycloak-as-admin](https://stackoverflow.com/questions/30622599/https-required-while-logging-in-to-keycloak-as-admin)



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


# Setup Kong api gateway
some references
[https://indocconsortium.atlassian.net/browse/IRDP-463](https://indocconsortium.atlassian.net/browse/IRDP-463)

Add a client in Keycloak
Go to Keycloak Admin UI. Go to Clients → Create to create a new client. Configure the following and save:

Client ID: kong

Client Protocol: openid-connect

Root URL: http://10.3.9.241:8000 (Kong proxy URL)

Accept Type: Confidential

Valid Redirect URIs: /* 

Go to tab Credentials and copy the client secret.


This ticket describes the workflow of how to make a request to an backend API protected by Kong API Gateway, which uses Keycloak as the identity provider through an OIDC plugin.

Setup

For details on how to set up the Keycloak client for Kong and configure Kong OIDC plugin, see above. We will use the setup where token introspection and bearer only are enabled so that it accepts jwt tokens and does not redirect to the login page.

In Kong, we have two services and two corresponding routes:

name

Kong proxy path

upstream service path

plugin

vre-portal-login

10.3.9.241:8000/vre/portal/users/auth 

10.3.9.240:5060/users/auth

vre-portal-api

10.3.9.241:8000/vre/portal

10.3.9.240:5060/ 

OIDC

By configuring the above, we do not need an access token to access the login endpoint but we need it for all other API endpoints. This works because Kong will choose the route with the longest path if multiple routes match the incoming request.

In Flask API, since the token validation is already taken care of by Kong OIDC plugin, we do not need to validate it again ourselves.

Workflow

Portal makes a request to the login endpoint exposed by Kong with the username and password provided by the user. Since this endpoint is not OIDC-protected, it does not require an access token. Kong proxies this request to the upstream service API, which exchanges the credentials with Keycloak for an access token. Portal then fetches and stores the access token returned.

For the subsequent API calls, portal sends the access token in the Authorization header along with the request. Since all other API endpoints are protected by the OIDC plugin, they all require an access token. Kong checks for the access token and validates it by talking to the token introspection endpoint of Keycloak. If the token is valid, Kong will proxy the request to the upstream service API and the result will be returned to portal. Otherwise, Kong will reject the request and return a 401 unauthorized error without proxying it to the upstream services.

How to set up new services:

In this example, I will describe how to add an upstream service, for example:
```
(base) $ curl http://10.3.9.240:5060/v1/datasets
{
    "result": [
        {
            "time_created": "2020-06-23T17:33:21",
            "id": 18,
            "name": "Carsten Finke Generate",
            "type": "Usecase",
            "time_lastmodified": "2020-06-23T17:33:21",
            "path": "/dataset/Carsten Finke Generate",
            "labels": [
                "Dataset"
            ]
        },
```
This is the API we want to put behind the kong API gateway. 

Step1: add a service called vre_test, the URL is the upstream service, in this case, is http://10.3.9.240:5060/v1/datasets

Step2: add a route, specify the http://10.3.9.241:8000/portal/v1/datasets and specify the method, in this case, is get

That’s all it needs to add a mapping. 

Test:

we can see now, the following does the same thing:
```
curl http://10.3.9.241:8000/portal/abc/
{
    "result": [
        {
            "time_created": "2020-06-23T17:33:21",
            "id": 18,
            "name": "Carsten Finke Generate",
            "type": "Usecase",
            "time_lastmodified": "2020-06-23T17:33:21",
            "path": "/dataset/Carsten Finke Generate",
            "labels": [
                "Dataset"
            ]
        },
```

add oidc authentication:

client id: kong

introspection endpoint: http://10.3.9.241:8080/auth/realms/testrealms/protocol/openid-connect/token/introspect

bearer only: yes

discovery: http://10.3.9.241:8080/auth/realms/testrealms/.well-known/openid-configuration

client secret: client secret copied from Keycloak

Test:
```
$ curl --header "Content-Type: application/json"   \
--request POST   \
--data '{"username":"testuser061501","password":"Trillian42!"}'  \
http://10.3.9.241:8000/portal/users/auth
{
    "result": {
        "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJrY3VRQjVCcDNTQlhRR0NpOVNCNDVjTGFnWnNRWVhkdi1OY3hQbzNJTU84In0.eyJleHAiOjE1OTMwMzcwNDIsImlhdCI6MTU5MzAzMzQ0MiwianRpIjoiNGE1ZjZjNjMtMzQwNi00MzdjLTgzYzEtY2UwZGE3ZGQ1MmIxIiwiaXNzIjoiaHR0cDovLzEwLjMuOS4yNDE6ODA4MC9hdXRoL3JlYWxtcy90ZXN0cmVhbG1zIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjI4NTI2ODY3LTAyY2MtNDlhZS1iNDA1LTE1NzA3ZmRiOGI0NSIsInR5cCI6IkJlYXJlciIsImF6cCI6ImtvbmciLCJzZXNzaW9uX3N0YXRlIjoiMTM3N2ZjMWQtMGY4Yi00MzhiLWIwN2MtM2FkZTIxOWRhMGEwIiwiYWNyIjoiMSIsInJlYWxtX2FjY2VzcyI6eyJyb2xlcyI6WyJvZmZsaW5lX2FjY2VzcyIsInVtYV9hdXRob3JpemF0aW9uIl19LCJyZXNvdXJjZV9hY2Nlc3MiOnsiYWNjb3VudCI6eyJyb2xlcyI6WyJtYW5hZ2UtYWNjb3VudCIsIm1hbmFnZS1hY2NvdW50LWxpbmtzIiwidmlldy1wcm9maWxlIl19fSwic2NvcGUiOiJwcm9maWxlIGVtYWlsIiwiZW1haWxfdmVyaWZpZWQiOnRydWUsIm5hbWUiOiJ0ZXN0dXNlcjA2MTUwMSB0ZXN0dXNlcjA2MTUwMSIsInByZWZlcnJlZF91c2VybmFtZSI6InRlc3R1c2VyMDYxNTAxIiwiZ2l2ZW5fbmFtZSI6InRlc3R1c2VyMDYxNTAxIiwiZmFtaWx5X25hbWUiOiJ0ZXN0dXNlcjA2MTUwMSJ9.hVetrxeLzZGD3hTqGuZgStH6m2WCgTGHG2DI_LQgL2VQnFci22bEyhbI52BxSUKC48uW-zlzmzwmPIX-zTr3UJPYM--j2UpMNHi2PeBUSX73q9RG09BY4eLkzT92JvaJFmiIsfYeX7Uh-pWUJPkFLyw1vSAXYdz-QbceutQKKOsymFNfcugiSYD3yDarD8J0PaygAfXvobDBlk5jF1VcaFIGIK7yvZtqBVFnKbUtR6PHEjgG1GehGNzCdBOZejuxUzomRRcPMekWeSuFeRVD1V6QYIGTvu029k1IhTOLCMVx-u8ucx3JdNTY27kFTQQUf2PtXkfW0kLCH8OWZCaIMQ",
        "token_type": "bearer",
        "refresh_expires_in": 1800,
        "not-before-policy": 0,
        "session_state": "1377fc1d-0f8b-438b-b07c-3ade219da0a0",
        "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJjNTQ3ZTc1ZC01MWZjLTQwMDMtOGI3Yy1lYmJlMDFkODRmOTAifQ.eyJleHAiOjE1OTMwMzUyNDIsImlhdCI6MTU5MzAzMzQ0MiwianRpIjoiY2Q5ZDFjZmMtZDJmZS00NmNlLThiNzgtOWRhZDhmYjcxY2QzIiwiaXNzIjoiaHR0cDovLzEwLjMuOS4yNDE6ODA4MC9hdXRoL3JlYWxtcy90ZXN0cmVhbG1zIiwiYXVkIjoiaHR0cDovLzEwLjMuOS4yNDE6ODA4MC9hdXRoL3JlYWxtcy90ZXN0cmVhbG1zIiwic3ViIjoiMjg1MjY4NjctMDJjYy00OWFlLWI0MDUtMTU3MDdmZGI4YjQ1IiwidHlwIjoiUmVmcmVzaCIsImF6cCI6ImtvbmciLCJzZXNzaW9uX3N0YXRlIjoiMTM3N2ZjMWQtMGY4Yi00MzhiLWIwN2MtM2FkZTIxOWRhMGEwIiwic2NvcGUiOiJwcm9maWxlIGVtYWlsIn0.5CLYFijJLirjYJnOPNfJDD_6nMVCJZMc4D5qxn1qIzU",
        "expires_in": 3600,
        "scope": "profile email"
    }
}

$ curl http://10.3.9.241:8000/portal/abc/
no Authorization header found
$ curl --header "Content-Type: application/json" \
--header "Authorization:Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJrY3VRQjVCcDNTQlhRR0NpOVNCNDVjTGFnWnNRWVhkdi1OY3hQbzNJTU84In0.eyJleHAiOjE1OTMwMzcwNDIsImlhdCI6MTU5MzAzMzQ0MiwianRpIjoiNGE1ZjZjNjMtMzQwNi00MzdjLTgzYzEtY2UwZGE3ZGQ1MmIxIiwiaXNzIjoiaHR0cDovLzEwLjMuOS4yNDE6ODA4MC9hdXRoL3JlYWxtcy90ZXN0cmVhbG1zIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjI4NTI2ODY3LTAyY2MtNDlhZS1iNDA1LTE1NzA3ZmRiOGI0NSIsInR5cCI6IkJlYXJlciIsImF6cCI6ImtvbmciLCJzZXNzaW9uX3N0YXRlIjoiMTM3N2ZjMWQtMGY4Yi00MzhiLWIwN2MtM2FkZTIxOWRhMGEwIiwiYWNyIjoiMSIsInJlYWxtX2FjY2VzcyI6eyJyb2xlcyI6WyJvZmZsaW5lX2FjY2VzcyIsInVtYV9hdXRob3JpemF0aW9uIl19LCJyZXNvdXJjZV9hY2Nlc3MiOnsiYWNjb3VudCI6eyJyb2xlcyI6WyJtYW5hZ2UtYWNjb3VudCIsIm1hbmFnZS1hY2NvdW50LWxpbmtzIiwidmlldy1wcm9maWxlIl19fSwic2NvcGUiOiJwcm9maWxlIGVtYWlsIiwiZW1haWxfdmVyaWZpZWQiOnRydWUsIm5hbWUiOiJ0ZXN0dXNlcjA2MTUwMSB0ZXN0dXNlcjA2MTUwMSIsInByZWZlcnJlZF91c2VybmFtZSI6InRlc3R1c2VyMDYxNTAxIiwiZ2l2ZW5fbmFtZSI6InRlc3R1c2VyMDYxNTAxIiwiZmFtaWx5X25hbWUiOiJ0ZXN0dXNlcjA2MTUwMSJ9.hVetrxeLzZGD3hTqGuZgStH6m2WCgTGHG2DI_LQgL2VQnFci22bEyhbI52BxSUKC48uW-zlzmzwmPIX-zTr3UJPYM--j2UpMNHi2PeBUSX73q9RG09BY4eLkzT92JvaJFmiIsfYeX7Uh-pWUJPkFLyw1vSAXYdz-QbceutQKKOsymFNfcugiSYD3yDarD8J0PaygAfXvobDBlk5jF1VcaFIGIK7yvZtqBVFnKbUtR6PHEjgG1GehGNzCdBOZejuxUzomRRcPMekWeSuFeRVD1V6QYIGTvu029k1IhTOLCMVx-u8ucx3JdNTY27kFTQQUf2PtXkfW0kLCH8OWZCaIMQ" \
http://10.3.9.241:8000/portal/abc/
{
    "result": [
        {
            "time_created": "2020-06-23T17:33:21",
            "id": 18,
            "name": "Carsten Finke Generate",
            "type": "Usecase",
            "time_lastmodified": "2020-06-23T17:33:21",
            "path": "/dataset/Carsten Finke Generate",
            "labels": [
                "Dataset"
            ]
```
