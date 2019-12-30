[https://github.com/RyanGao67/okta_react_node](https://github.com/RyanGao67/okta_react_node)   
ref:   
[https://developer.okta.com/blog/2018/02/06/build-user-registration-with-node-react-and-okta#add-routes-to-the-react-app](https://developer.okta.com/blog/2018/02/06/build-user-registration-with-node-react-and-okta#add-routes-to-the-react-app)


* cross domain requests were not allowed in browsers for security reasons for a long time(years ago)
* The cross-origin resource sharing policies started popping up in browers
* There are now good tools to enable specifically when you want to do a cross-domain post request to allow it from only certain place
* On my server I can say there places -- these origins are allowed to make cross origin request

* The whole way that the regular Oauth flow works is by doing a post request, the very last step is the app to do post request over to the token endpoint with authorization code that it has, and it gets back an access token. 
* That post request, if your OAuth server is on a different domain than your app, then that is a cross-origin request. That can only happen if the OAuth server says it is okay. 

* Way back then, if you want to do oauth, we'll let you cheat around the system and you do not need a client secret and you're going to do implicit flow and you use the client ID, and it returns the access token in the redirect from  the authorization server and bypasses that second step. 

* the simple route just return the access token, attack can happen, redirect can fail...
* When the access token is returned in the redirect step, if it is intercepted, it is game over

### OAuth2 and OIDC
* Roles defined in OAuth2
* Resource Owner(end user)
* Resource server(app/API controlling data)
* Client(app requesting data)
* Authorization server(app handling delegated authorization decision)

### Example 
* john is a developer who has been tole he needs to use OAth 2.0 to authorize users with an external server
* User + Application + API(authentication and source)

[https://www.youtube.com/watch?v=CPbvxxslDTU](https://www.youtube.com/watch?v=CPbvxxslDTU)

### Get started with Json web token
* json web token is an standard that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. 
* The information can be verified and trusted because it is digitally signed. (JWTs can be signed using a secret(with HMAC algorithm) or a public/private key pair using RSA)

* Compact: Because of its size, it can be sent through an URL, POST parameter, or inside an HTTP header. Additionally, due to its size its transmission is fast.
* Self-contained: The payload contains all the required information about the user, to avoid querying the database more than once.

* WHICH IS THE JSON WEB TOKEN STRUCTURE?
JWTs consist of three parts separated by dots (.), which are:

- Header
- Payload
- Signature
Therefore, a JWT typically looks like the following.

xxxxx.yyyyy.zzzzz

Let’s break down the different parts.

(1)Header
The header typically consists of two parts: the type of the token, which is JWT, and the hashing algorithm such as HMAC SHA256 or RSA.

For example:

{
  "alg": "HS256",
  "typ": "JWT"
}
Then, this JSON is Base64Url encoded to form the first part of the JWT.

(2) Payload
The second part of the token is the payload, which contains the claims. Claims are statements about an entity (typically, the user) and additional metadata. There are three types of claims: reserved, public, and private claims.

Reserved claims: These are a set of predefined claims, which are not mandatory but recommended, thought to provide a set of useful, interoperable claims. Some of them are: iss (issuer), exp (expiration time), sub (subject), aud (audience), among others.
Notice that the claim names are only three characters long as JWT is meant to be compact.

Public claims: These can be defined at will by those using JWTs. But to avoid collisions they should be defined in the IANA JSON Web Token Registry or be defined as a URI that contains a collision resistant namespace.
Private claims: These are the custom claims created to share information between parties that agree on using them.
An example of payload could be:

{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
The payload is then Base64Url encoded to form the second part of the JWT.

Signature
To create the signature part you have to take the encoded header, the encoded payload, a secret, the algorithm specified in the header, and sign that.

For example if you want to use the HMAC SHA256 algorithm, the signature will be created in the following way.

HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
The signature is used to verify that the sender of the JWT is who it says it is and to ensure that the message was’t changed in the way.

Putting all together
The output is three Base64 strings separated by dots that can be easily passed in HTML and HTTP environments, while being more compact compared to XML-based standards such as SAML.

[https://auth0.com/learn/json-web-tokens/](https://auth0.com/learn/json-web-tokens/)
