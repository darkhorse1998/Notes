# Scenarios

## Self Managed Cluster

Configuration wise self managed cluster is also fine because it would be an one time activity where you have to automate the deployments of all the components for Kubernetes. Once done, through IaC they can be deployed again and again if required.

| On-Prem | VM K8s | Managed |
| ------- | ------ | ------- |
| High maintenance required, upgrades are manual for both kubernetes version & OS upgrades of the servers. | High maintenance required, upgrades are manual for both kubernetes version though VMs might get upgraded by cloud provider. | Less maintenance as utomatic upgrades can be scheduled. |
| Costs can be lower or higher depending in situation. If you have unused infrastructure (servers, databases), as you might have moved the applications to cloud, you can use those to configure self managed clusters and lower the costs. You can probably use 3 servers as master nodes (control plane) and 2 as worker nodes (data plane). But, if you have to buy these resources from scratch then it will bump up your costs. | Costs mostly depends on how you are optimizing the resources, whether the VMs are of the right size, whether auto-scaling has been set. If they are not used properly, the costs would be higher as the resources will keep on running irrespecive of whether they are being used or not. | Costs can be significantly lower since the control plane would be managed by the cloud provider. |
| Scaling has to be manually done. | Auto-scaling can be set on the VMs. | Scalability can be ensured through auto-scaling of nodepools (maybe using VM ScaleSets). |
| Integration with other components like Ingress, CSI, secret management solutions are complicated. | Integrations are not very easy. | Integrations are easy. |
| Security is good as it is On-Prem. Good level of firewall control & security. | Security is not not good as it would be on public cloud. | Security is not not good as it would be on public cloud. |

> Source: [Self Managed vs Fully Managed Clusters](https://www.youtube.com/watch?v=o_7yvVqLZXQ)

## Autoscaling in Kubernetes

How are replicas decided by HPA? \
desiredReplicas = ceil[currentReplicas * ( currentMetricValue / desiredMetricValue )]

Usually we have seen HPA autoscaling pods based on CPU & memory utilizations. It could be defined as a percentage, where it will calculate the desired metrics based on the resource requests. It could also be defined in values. However, the scaling could also be based on custom metrics like number of HTTP requests per second, latency, queue depth etc. This can be achieved by leveraing a Custom Metric API server that collects these metrics so that the HPA could use these metrics and scale accordingly. \
During scaling we often observe that the pods might be scaling up and down with some delays. This is because Kubernetes autoscaling has intentional delays by default, so that the system doesn't scale up or down too quickly due to spurious load. However, we can control this through **scaleUp** or **scaleDown** beaviors. Following is the configuration and the result:

```yaml
behavior:
    scaleDown:
      stabilizationWindowSeconds: 0
    scaleUp:
      stabilizationWindowSeconds: 0
```
![image](https://github.com/user-attachments/assets/3880bcff-9a5a-408a-beab-145e43c13840)

> Source: [Autoscaling with Custom Metrics](https://blog.px.dev/autoscaling-custom-k8s-metric/)

## Graceful Shutdown

These are critical in cases where you might have a long running processes. Possible issues could be:

1. The pod could be performing a long running porcess when it gets stopped due to pod termination. This long running process would need to be started again from scratch, leading to waste of resources & time.
2. The pod deletion or termination process and the endpoints update process are started parallely. It could be possible that the pod gets deleted first before the endpoints can be updated in verious kubernetes components like ingress, CoreDNS, kubeproxy etc. In this case, request would continue to come in an dreceive a 404 as the pod won't be there.

How can they be tackled to some extent?
To allow long running processes to complete, the `terminationGracePeriodSeconds` can be extended. This will ensure that even after receiving the **SIGTERM** signal and pod being in **terminating** state, the process still get the time to complete. Some new requests might come in if there are some delays in the endpoint update in the kubernetes components. To prevent this, one can configure a `preStop` hook to run when the pod receives a **SIGTERM** signal, which can run some scripts or commands to set some environment variables, based on which requests are accepted or denied by the application. This should take care of #1 to some extent.

```yaml
lifecycle:
  preStop:
    exec:
      command: ["<run-script>"]
      terminationGracePeriodSeconds: <required-time>
```

#2 can be solved through a similar manner. In case there are no process running after receiving the **SIGTERM** signal, the pod might terminate well before the default grace period or before the endpoints could be updated in the Kubernetes components. A delay can be introduced through the `preStop` hook to ensure that the pod don't terminate before the endpoints are updated.

```yaml
lifecycle:
  preStop:
    exec:
      command: ["sleep", "15"]
```

> Source: [Graceful Shutdown](https://learnk8s.io/graceful-shutdown)