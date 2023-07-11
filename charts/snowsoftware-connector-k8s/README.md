# Snow Software Kubernetes Container Connector

Container connectors enable data to be collected and transferred from your Kubernetes environment to Snow Atlas. You create new container connectors in Snow Atlas and use this Helm chart to install them in your clusters.

For container connectors to collect data from your Kubernetes clusters, you must create and install a connector for each cluster.

## Prerequisites

- Kubernetes 1.23+
- Helm 3+
- Outbound network access on port 443
- SecretKey generated in Snow Atlas for container connector (See [Set up container connectors](https://docs.snowsoftware.com/snow-atlas/en/UUID-87f651b5-485d-016a-f80a-ece675a3d24b.html))

## Cpu and memory resources

The default and recommended values for the CPU and memory resources are shown in the table below. If you require to modify these values, update the values.yaml file.

|               |  CPU for requests | Memory requests | Memory limits |
|---------------|:-----------------:|:-------------------:|:-----------------:|
| Agents        |  100 m            | 50  MiB             | 250 MiB           |
| Aggregators   |  300 m            | 100 MiB             | 250 MiB           |
| Message queue |  300 m            | 100 MiB             | 250 MiB           |

## Install container connectors with default values

```
helm repo add snowsoftware https://snowsoftwareglobal.github.io/helm-charts
helm repo update

helm install snowsoftware-connector snowsoftware/snowsoftware-connector-k8s --set secretKey=[SecretKey]
```
## Install container connectors with custom certificates

By default this chart generates Secrets for your secretkey and ssl certificates. The ssl certificates are used for internal communication only and are never accessible outside your cluster. 

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

helm install snow-connector -f values.yaml snowsoftware/snowsoftware-connector-k8s
```

## Install container conectors with custom namespace

Use this procedure if you want to include a custom namespace when you install the Helm chart.
When you install the snowsoftware-connector-k8s Helm chart from the repository github.com/SnowSoftwareGlobal/helm-charts, you require the secret key that you generate and copy in [Create container connectors](https://docs.snowsoftware.com/snow-atlas/en/UUID-87f651b5-485d-016a-f80a-ece675a3d24b.html#UUID-87f651b5-485d-016a-f80a-ece675a3d24b_section-idm4653764694574432443814737848).

  Add the repository [https://snowsoftwareglobal.github.io/helm-charts](https://snowsoftwareglobal.github.io/helm-charts) to your Helm chart repositories:

```
      helm repo add snowsoftware https://snowsoftwareglobal.github.io/helm-charts
      helm repo update
```

Install the connector with the latest version, enter the namespace, and paste in the secret key that you generated:

```
      helm install -n [namespace] snowsoftware-connector snowsoftware/snowsoftware-connector-k8s --set secretKey=[SecretKey]
```

## Upgrade container connectors
Load all the new charts from the repository [github.com/SnowSoftwareGlobal/helm-charts](github.com/SnowSoftwareGlobal/helm-charts):

```
helm repo update
```

### View the charts available:

```
helm search repo
```

List and verify what is currently installed:

```
helm list
```

### Upgrade to a specific version:

```
helm upgrade snowsofware-connector snowsoftware/snowsoftware-connector-k8s --version [release version]
```

### Upgrade to the latest version:

```
helm upgrade snowsoftware-connector snowsoftware/snowsoftware-connector-k8s
```

**NOTE**
If you make changes to the values.yaml file, you must upgrade the helm charts to apply your changes:
```
helm upgrade snow-connector -f values.yaml snowsoftware/snowsoftware-connector-k8
```