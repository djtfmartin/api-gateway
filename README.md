# Atlas API Gateway

This repository is an issues & documentation repository for managing the delivery of the API Gateway for the Atlas.

The API gateway will provide a **single entry point** for all Atlas webservice services and will include an **enhanced authentication model** for web service users.
There will be two types of access supported: 1) OIDC access 2) Non-OIDC access

## OIDC access

For browser or mobile applications based applications that can make use of OAuth SDKs or Spring Security to generate a JWT.

<img width="999" alt="Screen Shot 2021-11-02 at 12 02 19 pm" src="https://user-images.githubusercontent.com/444897/139842638-e96e1d4b-da73-48ea-b1db-56cd4f5e1e4a.png">

## Non-OIDC based access

For applications that are not using browser, there will be services allowing for the creation of JSON Web Tokens either by using Basic Authentication or an API Key/Secret mechanism (yet to be determined). Once the JWT is generated, it will be submitted with an requests in the ```Authorization``` header. 

<img width="815" alt="Screen Shot 2021-11-02 at 12 02 30 pm" src="https://user-images.githubusercontent.com/444897/139842663-ad70c594-f11e-4586-b8aa-7b25d2de6739.png">

# Authentication flow

All requests will require both a JWT and an API Key.

### API key
The API key is used for monitoring only and will be only used/checked at the Gateway level. The backend webservices will not make use of this API key for authorisation or authentication.

### JSON Web Token (JWT)
The JSON web token will contain user ID and Roles. The JWT will be accessible to backend webservices. This will allow the backend end webservices to apply their own access control based on roles and/or user ID.  
#### Examples of app-based access control: 
* project level access in Ecodata
* sensitive data access for a specific region in the biocache service.

#### JWT Verification and accessing roles/userID
The verification of the JWT and extraction of userID and roles will be done by a Grails plugin for grails based applications which should limited the amount of JWT logic spread in Atlas applications.

<img width="297" alt="Screen Shot 2021-11-02 at 12 03 02 pm" src="https://user-images.githubusercontent.com/444897/139842699-d146a824-7856-400e-9c3b-444df6fee211.png">


### Example JWT

A JSON Web Token is signed allowing the backend webservices to verify its source using the signature. The unpacked contents of the JWT will look this:

```json

{
  "country": "AU",
  "role": [
    "ROLE_COLLECTIONS_ADMIN",
    "ROLE_USER"
  ],
  "city": "Sydney",
  "iss": "auth.ala.org.au",
  "organisation": "MyOrg",
  "given_name": "Derek",
  "userid": 43956,
  "aud": "oidc-test-client-id",
  "nbf": 1635854927,
  "authority": "ROLE_COLLECTIONS_ADMIN,ROLE_USER",
  "state": "NSW",
  "exp": 1635854927,
  "iat": 1635853127,
  "family_name": "Smith",
  "email": "my@email.com"
}

```


# Legacy API Keys

The current system for application to application authentication which uses the [apikey](https://github.com/atlasoflivingaustralia/apikey) which is only used internally (i.e. not accessible for the third parties) will be phased out over time.
Support in the short term for legacy API keys will continue (through the ala-ws-security-plugin) but will gradually be phased out in favour of JWT support.



