# keycloakNestedTokenExtension

keycloak plugin to enrich token with another token

## Sumary

  - [Sumary](#sumary)
  - [Description](#description)
  - [Decoded token sample](#decoded-token-sample)
  - [Start keycloak](#start-keycloak)
  - [configure a client credentials](#configure-a-client-credentials)
  - [Install plugin](#install-plugin)
  - [Test](#test)

## Description
keycloak plugin to create a Nested jwt token. Nested JWT is a JWT in which the payload is another JWT

## Decoded token sample
```json
{
  "alg": "HS256",
  "typ": "JWT",
}

{
  "sub": "1234567890",
  "name": "John Doe",
  "iat": 1516239022,
  "njwt": "<njwt>"
}
```

## Start keycloak
execute this command to start a keycloak server
```
docker run -p 8080:8080 -e KEYCLOAK_USER=admin -e KEYCLOAK_PASSWORD=admin quay.io/keycloak/keycloak:14.0.0
```

## configure a client credentials

read this [tutorial](https://www.appsdeveloperblog.com/keycloak-client-credentials-grant-example/)

## Install plugin

clone this repository
```
https://github.com/caiowWillian/keycloakNestedTokenExtension.git
```

at the root build the application
```
mvn clean package
```

at the path ./target execute this commando
```
docker cp keycloak-customProtocolMapper-1.0-SNAPSHOT.jar <container_id>:/opt/jboss/keycloak/standalone/deployments/keycloak-customProtocolMapper-1.0-SNAPSHOT.jar
```

## Test 
Add mapper in your client and make a get token
```
curl --location --request POST 'http://localhost:8080/auth/realms/master/protocol/openid-connect/token' \
--header 'njwt: <your nested token>` \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'client_id=<your client_id>' \
--data-urlencode 'client_secret=<your_client_secret' \
--data-urlencode 'grant_type=client_credentials' \

```