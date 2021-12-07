# Datadog

Deployment using the operator [fails](https://github.com/DataDog/datadog-operator/issues/415) so we use the DaemonSet
deployment.

* create secret with API and app
  keys: `kubectl create secret generic datadog-secret --from-literal api-key=<api key> --from-literal app-key=<app key>`

## Cluster agent

https://docs.datadoghq.com/agent/cluster_agent/setup/?tab=daemonset

* replace `namespace: monitoring` by `namespace: monitoring`
* use `datadog-secret` for API and app keys.
* add `securityContext.runAsNonRoot: false` to cluster agent deployment
* add `DD_CLUSTER_NAME=esxi-01`
* follow https://docs.datadoghq.com/agent/cluster_agent/external_metrics/?tab=daemonset

## Agent

https://docs.datadoghq.com/agent/kubernetes/?tab=daemonset
choose metrics+logs+apm

* replace `namespace: monitoring` by `namespace: monitoring`
* use `datadog-secret` for API key
* add toleration for master node
* add `securityContext.runAsNonRoot: false`
* set `DD_KUBELET_TLS_VERIFY=false` on all three containers
* add `DD_HOSTNAME` (from `spec.nodeName`) on all three containers
* follow https://docs.datadoghq.com/agent/cluster_agent/setup/?tab=daemonset#configure-the-datadog-agent
* follow https://docs.datadoghq.com/infrastructure/livecontainers/configuration/?tab=daemonset and
  add `DD_CLUSTER_NAME=esxi-01` to process-agent
