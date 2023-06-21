# Snow Software Tokenbroker Proxy
This chart installs the tokenbroker proxy service on the client's cluster. 
This service is responsible for obtaining a token that is used to communicate with Snow Atlas based resource APIs. 

The service converts between Mutual TLS (MTLS) and OAuth2 based authentication. To do this, it has to be provided with an MTLS certificate chain.

These are all of the parameters the service requires:
* Client ID - Client ID registered in Snow Identity Provider in OicdApplication
* Client Secret - Client Secret registered in Snow Identity Provider in OicdApplication
* Server Certificate - SSL certificate used by the tokenbroker proxy service
* Signing Certificate - Certificate for signing the token
* Certificate Authority - Client certificate authority
* Platform URL - URL to Snow Identity Provider
* BrokerPort - Port on which the tokenbroker proxy service rus
* BrokerHost - Host name for tokenbroker proxy service


# Required Software
helm
https://helm.sh/docs/intro/install/

kubectl
https://kubernetes.io/docs/tasks/tools/



# Usage
## Prerequisites
1. Add Snow Software Helm Charts repository:
```
helm repo add snowsoftware https://snowsoftwareglobal.github.io/helm-charts
helm repo update
```
2. Prepare required certificates
   - clientca.crt
   - server.pem
   - signingcert.pem
3. Prepare cliendId and clientSecret. Obtain the credentials by registering a token broker proxy application in Snow Atlas.
## Installation
1. Install secrets and certificates on the cluster

   To provide required certificates and secrets to tokenbroker proxy service, (Server Certificate, Signing Certificate, Certificate Authority, Client Id, Client secret) on the cluster using Kubernetes Secrets, create secret.yaml file:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: snowsoftware-tokenbroker-proxy-secrets
type: Opaque
data:
  clientid: <base64-encoded client id>
  clientsecret: <base64-encoded client secret>
  clientca: <base64-encoded client CA>
  servercert: <base64-encoded server cert>
  signingcert: <base64-encoded signing cert>
```
Replace the placeholders with the base64 encoded values of client id and secrets and certificate files contents.

Apply the secrets to your cluster:
```
kubectl apply -f secret.yaml
```
2. Prepare values.yaml file

   When installing the chart, you will have to provide 3 additional values: platformurl, brokerhost and brokerport. 
   Create a values.yaml file to override the rest of the values:
```yaml
tokenbrokerProxy:
  platformurl: "<snow atlas identity provider URL>"
  brokerport: "8443" # default
  brokerhost: "localhost" # default
```

3. Run helm installation and provide the values.yaml file you just created (example using downloaded file)
```
helm install snowsoftware-tokenbroker-proxy snowsoftware/snowsoftware-tokenbroker-proxy -f values.yaml
```

