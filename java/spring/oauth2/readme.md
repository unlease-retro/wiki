# OAuth2

We use **[OAuth2](http://oauth.net/2/)** with **[JWT](https://jwt.io/)** at unlease :pineapple: to achieve stateless and scalable user authentication and authorization. The implementation is using **[Spring Security OAuth2](http://projects.spring.io/spring-security-oauth/docs/oauth2.html)** 

By using **JWT** and **separated Auth and Resource server**, there's **no need** for Resource Server to verify access token with Auth Server, which makes the whole user authentication and authorization very light weight and stateless.

Using OAuth2 gives us the possibility to use any language / framework as resource server as long as it has OAuth2 and JWT support to develop our backend endpoints  

## Hopefully the examples below still works :ramen:

- [Auth server example](https://github.com/csueaq/springBoot-oauth-jwt-tokenStore-authServer)
- [resource server example](https://github.com/csueaq/springBoot-oauth-jwt-tokenStore-resourceServer)

## Generate RSA key pair 

- Create java keystore(.jks) file:

```bash
$ keytool -genkeypair -alias <YOURKEY> -keyalg RSA \
-dname "CN=Web Server,OU=Unit,O=Organization,L=City,S=State,C=US" \
-keypass <YOURKEYPASS> -keystore server.jks -storepass <YOURSOTREPASS>
```

- Export public key certificate file

```bash
$ keytool -export -keystore server.jks -alias <YOURKEY> -file someCert.cer
```

- print the public key

```bash
$ openssl x509 -inform der -in someCert.cer -pubkey -noout

-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA3Kd1vQNTLHLVhyMR0JHj
Q3CxJ9Roi6aZTzUk/HDerxJ+1ey8CdX4zf8bFA9Fh21KTojw87yt76A6GpCuru6P
zxCou0GLPwFwKCS1SFcsysOMSxRAhgIssjujGnbC2Q0XPDpsGYJVavnHGZ7cI7Hn
sXqHcL0dmbgEfI7NR7wCGHoo1NxjfwOQXtCGH3w/Tg2BLA3HNyRclrCfJuS3aj0y
tr7tOWdzgguztH6E4xoqKdn7FEMMtBEsggw7Z4H8uziUy37Z7iOMTdmwZvbpMrns
IUZElqnYcRFYLPRH5xsSl1Y129fAbW03WW63agzy9DWO5HhT44ePJDrkZqsEaHKw
/QIDAQAB
-----END PUBLIC KEY-----

```

## Using the keys

Once you have **server.jks** and **public key** , 

- you need to config your **Auth Server** to use **server.jks** in [AuthorizationServer configuration](https://github.com/csueaq/springBoot-oauth-jwt-tokenStore-authServer/blob/master/src/main/java/authenticationServer/AuthorizationServer.java)

- you need to configure your **Resource Server** to use a **public key** to verify access token, if you use **Spring Boot 1.3+** , then put it in [application.yml](https://github.com/csueaq/springBoot-oauth-jwt-tokenStore-resourceServer/blob/master/src/main/resources/application.yml)

## Gotcha 

#### Spring Security OAuth2 return code
-  401 when **access token expired** and **cannot convert access token to JSON** and **no access token found**
-  400 when **bad credentials** i.e wrong password :expressionless:
-  403 sometimes can't remember when :disappointed:

#### JWT Token

- There is **no way** to revoke a valid access token until it expires, so use **short ttl** for access token and **long ttl** for refresh token which we can prevent new access token to be issued for refreshing access token
- It is better to **lower the permission (scope)** after a certain time when access token is first issued (can be achieved when refreshing access token), so we can re-verify user identity on some sensitive actions if they logged in long time ago

#### User DAO object

- Using separated Auth and Resource server will ended up **duplicating** the User DAO object, which is very annoying when adding new field to it if directly use JPA **save** method to process update, as it will **overwrite** the entire document/row. Therefore, it is best to use **explicit update** on the field to avoid constantly keep both DAO objects in sync.

## Reference

- [Key generation](https://beku8.wordpress.com/2015/03/31/configuring-spring-oauth2-with-jwt-asymmetric-rsa-keypair/) 
