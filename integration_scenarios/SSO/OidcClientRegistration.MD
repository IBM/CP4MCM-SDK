# Register an OIDC Client Using the OidcClientRegistration Custom-Resource
## Example
```
apiVersion: integrations.sdk.cp4mcm.ibm.com/v1beta1
kind: OidcClientRegistration
metadata:
  name: oidcclientregistration-sample
  namespace: default
spec:
  registration:
    post_logout_redirect_uris:
      - https://ibm.com
    trusted_uri_prefixes:
      - https://ibm.com
    redirect_uris:
      - https://ibm.com
    client_secret:
      secretName: client-reg-secret
      secretKey: client_secret
    apply_client_secret: true
```
This example shows the minimum set of values required to create the oidc-client registration. For information about all available oidc-client options see documentation [here](https://www.ibm.com/support/knowledgecenter/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/twlp_client_registration.html).
## Managing the Client-Secret in Kubernetes
In the above example, these fields define the Kubernetes secret that will hold the value of the oidc-client secret:
```
client_secret:
  secretName: client-reg-secret
  secretKey: client_secret
```
When `apply_client_secret` is set to `true` a new Kubernetes secret will automatically be created in the same namespace as the OidcClientRegistration, which will contain the automatically generated client secret in the key specified in `secretKey`. When `apply_client_secret` is set to `false`, the caller is expected to create and manage this Kubernetes secret instead. The provided value in the specified key will be registered as the client-secret for the new OIDC client.
## Acquiring the ClientID
Once the custom-resource has been reconciled a ClientID will automatically be chosen for it. The ClientID should follow this format: `sdk.cp4mcm.ibm.com.{{ .metadata.namespace }}.{{ .metadata.name }}`, where the namespace and name refer to the OidcClientRegistration resource in Kubernetes. The ClientID will also appear in the `.status.state` field of the resource once it has been reconciled. This can be verified like so:
```
$ oc get oidc oidcclientregistration-sample

NAME                            STATUS       CLIENT_ID
oidcclientregistration-sample   CONFIGURED   sdk.cp4mcm.ibm.com.default.oidcclientregistration-sample
```