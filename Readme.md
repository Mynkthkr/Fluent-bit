### TABLE OF CONTENTS

[1-What is Helm Chart? ](https://github.com/Mynkthkr/stack/new/main#what-is-helm-chart)

[2-Helm Chart Structure](https://github.com/Mynkthkr/stack/new/main#helm-chart-structure)

[3-Deploy the Helm Chart](https://github.com/Mynkthkr/stack/new/main#deploy-the-helm-chart)

[4-Helm Upgrade & Rollback](https://github.com/Mynkthkr/stack/new/main#helm-upgrade--rollback)

[5-Uninstall The Helm Release](https://github.com/Mynkthkr/stack/new/main#uninstall-the-helm-release)


## What is Helm Chart?

For the purpose of explanation, I am choosing example, suppose we have an application which to be deploy on kubernetes
Let’s assume you have three different environments in your project. Dev, QA, and Prod. Each environment will have different parameters for application. For example,


In Dev and QA you might need only one replica.


In production, you will have more replicas with pod autoscaling.


The ingress routing rules will be different in each environment.


The config and secrets will be different for each environment.

## Helm Chart Structure

To understand the Helm chart, let’s take an example of Fluentbit. To deploy fluentbit on Kubernetes, typically you would have the following YAML files.
```
fluent-bit
    ├── daemonset.yaml
    ├── config.yaml
    └── rbac.yaml
```

Now if we create a Helm Chart for the above Fluentbit, it will have the following directory structure.

```
fluentbit-chart/
|-- Chart.yaml
|-- templates
|   |-- NOTES.txt
|   |-- _helpers.tpl
|   |-- daemonset.yaml
|   |-- config.yaml
|   |-- rbac.yaml

`-- values.yaml
```
## Deploy the Helm Chart

When you deploy the chart, Helm will read the chart and configuration values from the values.yaml file and generates the manifest files.

We need to changes the values accordingly in values.yaml

```
daemonset:
  env:
    FLUENT_ELASTICSEARCH_HOST: "172.31.30.23"
    FLUENT_ELASTICSEARCH_PORT: "9200"

config:
  output:
    ELASTICSEARCH_IP: "172.31.30.23"
    
env:
  name: "qa"
```

Now we are ready to install the chart.


Execute the following command where ``` fluent-bit ``` is release name and ```fluentbit-helm-chart``` is the chart name. It installs ```fluentbit-helm-chart``` in the default namespace

```
helm install fluent-bit fluentbit-helm-chart
```

You will see the output as shown below.

```
zxc
```

Now you can check the release list using this command:

```
helm list
```

## Helm Upgrade & Rollback

Now suppose you want to modify the chart and install the updated version, we can use the below command:

```
helm upgrade fluent-bit fluentbit-helm-chart
```

If we want to roll back the changes which we have just done and deploy the previous one again, we can use the rollback command to do that.

```
helm rollback fluent-bit
```

## Uninstall The Helm Release

To uninstall the helm release use uninstall command. It will remove all of the resources associated with the last release of the chart.

```
helm uninstall fluent-bit
```











