# Atlas API Gateway

This repository is an issues & documentation repository for managing the delivery of the API Gateway for the Atlas.

The API gateway will provide a **single entry point** for all Atlas webservice services and will include an **enhanced authentication model** for web service users.
There will be two types of access supported.

## OAuth access

For browser or mobile application based applications.

<img width="1079" alt="Screen Shot 2021-11-01 at 8 59 58 am" src="https://user-images.githubusercontent.com/444897/139836601-3203bdc0-678e-4460-a5da-cca336d1e3eb.png">

## Non-OAuth access

For applications that are not using browser.

<img width="911" alt="Screen Shot 2021-11-01 at 8 59 41 am" src="https://user-images.githubusercontent.com/444897/139836615-c65e4945-afcd-45a7-bbd7-13b884d34832.png">

# Authentication flow

All requests will require both a JWT and an API Key.

### API key
The API key is used for monitoring only and will be only used/checked at the Gateway level. The backend webservices will not make use of this API key for authorisation or authentication.

### JSON Web Token (JWT)
The JSON web token will contain user ID and Roles. The JWT will be accessible to backend webservices. This will allow the backend end webservices to apply their own access control based on roles and/or user ID. 

#### Examples: 
* project level access in Ecodata
* sensitive data access for a specific region in the biocache service.

<img width="477" alt="Screen Shot 2021-10-31 at 3 30 23 pm" src="https://user-images.githubusercontent.com/444897/139836634-36865255-021a-4973-8e0c-8a865f247146.png">

### Example JWT

A JSON Web Token is signed allowing the backend webservices to verify its source using the signature. The unpacked contents of the JWT will look this:

```json

{
  "country": "AU",
  "role": [
    "ROLE_ADMIN",
    "ROLE_USER"
  ],
  "city": "Sydney",
  "iss": "auth.ala.org.au",
  "organisation": "MyOrg",
  "given_name": "David",
  "userid": 43956,
  "aud": "oidc-test-client-id",
  "nbf": 1635854927,
  "authority": "ROLE_ADMIN,ROLE_USER",
  "state": "NSW",
  "exp": 1635854927,
  "iat": 1635853127,
  "family_name": "Martin",
  "email": "my@email.com"
}

```


# Legacy API Keys

The current system for application to application authentication which uses the [apikey](https://github.com/atlasoflivingaustralia/apikey) which is only used internally (i.e. not accessible for the third parties) will be phased out over time.
