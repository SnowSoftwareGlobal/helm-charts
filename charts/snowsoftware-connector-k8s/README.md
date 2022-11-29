# Snow Software Kubernetes Container Connector

Container connectors enable data to be collected and transferred from your Kubernetes environment to Snow Atlas. You create new container connectors in Snow Atlas and use this Helm chart to install them in your clusters.

For container connectors to collect data from your Kubernetes clusters, you must create and install a connector for each cluster.

## Prerequisites

- Kubernetes 1.23+
- Helm 3+
- Outbound network access on port 443
- SecretKey generated in Snow Atlas for container connector (See [Set up container connectors](https://docs.snowsoftware.com/snow-atlas/en/UUID-87f651b5-485d-016a-f80a-ece675a3d24b.html))

## Install container connectors with default values

```
helm repo add snowsoftware https://snowsoftwareglobal.github.io/helm-charts
helm repo update

helm install snowsoftware-connector snowsoftware/snowsoftware-connector-k8s --set secretKey=[SecretKey]
```
## Install container connectors with custom certificates

By default this chart generates Secrets for your secretkey and ssl certificates. The ssl certificates are used for internal communication only and are never be accessible outside your cluster. 

Use this procedure if you want to create and manage these Secrets manually.  The ssl certificate Secrets must include entries for a tls.crt and tls.key for the agent, aggregator and message queue.

```
helm repo add snowsoftware https://snowsoftwareglobal.github.io/helm-charts
helm repo update

helm install snowsoftware-connector snowsoftware/snowsoftware-connector-k8s \
       --set existingSecretKey=[name of secretkey in k8s] \
       --set existingAgentCertificate=[name of agent certificate secret in k8s] \
       --set existingAggregatorCertificate=[name of agent certificate secret in k8s] \
       --set existingMQCertificate=[name of agent certificate secret in k8s]
```

## Install container connectors with amended values

Copy the values.yaml file from https://github.com/SnowSoftwareGlobal/helm-charts/blob/main/charts/snowsoftware-connector-k8s/values.yaml and amend it as you require.

```
helm repo add snowsoftware https://snowsoftwareglobal.github.io/helm-charts
helm repo update

helm install snow-connector \ 
      -f values.yaml \
      snowsoftware/snowsoftware-connector-k8s
```

## Upgrade container connectors
Load all the new charts from the repository github.com/SnowSoftwareGlobal/helm-charts:

```
helm repo update
```

View the charts available:

```
helm search repo
```

List and verify what is currently installed:

```
helm list
```

Do one of the following:

Upgrade to a specific version:

```
helm upgrade snowsofware-connector snowsoftware/snowsoftware-connector-k8s \
      --version [release version]
```

Upgrade to the latest version:

```
helm upgrade snowsoftware-connector snowsoftware/snowsoftware-connector-k8s
```