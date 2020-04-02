# Single Sign On Scenario
This is an integration scenario where link(s) to business partner's app have already been embedded into IBM MCM UI. When an user clicks on these links the user should not be prompted to login again on the business partner's website. This is a single sign on (SSO) scenario. Business partner apps can implement SSO with MCM.  This document describes an example SSO implementation using a simple sample application. This sample app has the role of OIDC Relying Party (RP) and MCM has the role of OIDC provider (OP) in this SSO scenario.
## Prerequisites
IBM MCM support OIDC and act as an OIDC provider (OP) in the SSO scenario.  Business partner’s application should support OIDC and act as an OIDC relying party (RP) during the SSO process. If you are not familiar with OIDC, this article can help with understanding the concept of OIDC. https://www.ibm.com/developerworks/websphere/library/techarticles/1502_odonnell/1502_odonnell.html
## Implementing SSO with IBM MCM Short Description
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
