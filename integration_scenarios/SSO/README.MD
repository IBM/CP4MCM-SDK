# Single Sign On Scenario
This is an integration scenario where link(s) to business partner's app have already been embedded into IBM MCM UI. When an user clicks on these links the user should not be prompted to login again on the business partner's website. This is a single sign on (SSO) scenario. Business partner apps can implement SSO with MCM.  If you are experienced with OIDC you can follow the short description steps.  Otherwise this document describes an example SSO implementation using a simple sample application. This sample app has the role of OIDC Relying Party (RP) and MCM has the role of OIDC provider (OP) in this SSO scenario.
## Prerequisites
IBM MCM support OIDC and act as an OIDC provider (OP) in the SSO scenario.  Business partner’s application should support OIDC and act as an OIDC relying party (RP) during the SSO process. If you are not familiar with OIDC, this article can help with understanding the concept of OIDC. https://www.ibm.com/developerworks/websphere/library/techarticles/1502_odonnell/1502_odonnell.html
## Implementing SSO with IBM MCM 
### Short Description
If you are already familiar with OIDC, implementing SSO for business partner app with IBM MCM has just 3 steps
1. Configure OIDC client (RP) in business partner's app to point to IBM MCM OP's discovery endpoint.  For example:
```
<openidConnectClient id="oidcclient"
    scope="openid profile"
    clientId="oidcclient"
    clientSecret="password"
    discoveryEndpointUrl="https://<MCM hostname>/oidc/endpoint/OP/.well-known/openid-configuration">
</openidConnectClient>
```
2. Create a registration json from the registration template.  Configure the registration json with business partner app RP's client's clientID, client secret, redirect url and trusted uri prefix.  See registration template in this document: https://www.ibm.com/support/knowledgecenter/SSFC4F_1.2.0/iam/3.4.0/auth_onboard.html
3. Log into MCM from command console then execute command `cloudctl iam oauth-client-register -f registration.json`
That is it.
### Implementing SSO Example with Sample App
The Sample app is a simple one-page application.  This application is deployed as a war file to an IBM WebSphere Liberty server.  The Liberty server is configured as an OIDC client (RP).  Implementing SSO of the Sample app with MCM consist of these steps:
1. The configuratiton of Liberty server as a OIDC client is done by editing the server.xml.
```
<openidConnectClient authorizationEndpointUrl="https://<MCM PROXY IP>:443/idprovider/v1/auth/authorize" 
    clientId="sdksample" 
    clientSecret="password"
    id="sdksample" 
    tokenEndpointUrl="https://<MCM PROXY IP>:443/idprovider/v1/auth/token" 
    issuerIdentifier="https://<MCM PROXY IP>:443/idauth/oidc/endpoint/OP"  
    jwkEndpointUrl="https://<MCM PROXY IP>:443/oidc/endpoint/OP/jwk"
    signatureAlgorithm="RS256" >
</openidConnectClient>
Alternatively, you can use OP’s discovery endpoint for configuration
<openidConnectClient id="oidcclient"
    scope="openid profile"
    clientId="oidcclient"
    clientSecret="password"
    discoveryEndpointUrl="https://<MCM PROXY IP>/oidc/endpoint/OP/.well-known/openid-configuration">
</openidConnectClient>
```
  [link to sample server.xml](on-prem_liberty/server.xml)

2. Build and deploy the Sample app to the configured Liberty server
3. Create RP registration.json from template.  See registration template in this document: https://www.ibm.com/support/knowledgecenter/SSFC4F_1.2.0/iam/3.4.0/auth_onboard.html

```
{
  "token_endpoint_auth_method": "client_secret_basic",
  "client_id": "sdksample",
  "client_secret": "password",
  "scope": "openid profile email",
  "grant_types": [
    "authorization_code",
    "client_credentials",
    "password",
    "implicit",
    "refresh_token",
    "urn:ietf:params:oauth:grant-type:jwt-bearer"
  ],
  "response_types": [
    "code",
    "token",
    "id_token token"
  ],
  "application_type": "web",
  "subject_type": "public",
  "post_logout_redirect_uris": [
    "https://<MCM PROXY IP>:443/console/logout"
  ],
  "preauthorized_scope": "openid profile email general",
  "introspect_tokens": true,
  "trusted_uri_prefixes": [
      "https://<SAMPLE APP URL>/"
  ],
  "redirect_uris": [
    "https://<SAMPLE APP HOST>/oidcclient/redirect/sdksample"
  ]
}
```
4. Log into MCM from command console then execute command `cloudctl iam oauth-client-register -f registration.json`

