
# NetCorePal Ingress Controller

## How to install



```shell
helm repo add netcorepal https://netcorepal.github.io/helm-charts/
helm repo update

# Install a `NetCorePal Ingress Controller` use release name `myrelease`.
helm install myrelease netcorepal/netcorepal-ingress-controller   
```

## Ingress Sample

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: backend-service-name
  namespace: default
spec:
  ingressClassName: myrelease  # Default use your helm release name 
  rules:
  - host: backend.com
    http:
      paths:
      - backend:
          service:
            name: backend-service-name
            port:
              number: 80
        path: /
        pathType: ImplementationSpecific

```

## Chart Configuration Options

|Parameter|Description|Default|
|--|--|--|
|`replicaCount`|The replicaCount for `NetCorePal Ingress Controller` Container.|`1`|
|`image.repository`|The fullname of image for `NetCorePal Ingress Controller` Container.|`ghcr.io/netcorepal/netcorepal-ingress-controller`|
|`image.pullPolicy`|The image pull policy for `NetCorePal Ingress Controller` Container.|`IfNotPresent`|
|`image.tag`|The version/tag to use for `NetCorePal Ingress Controller` Container.|`latest`|
|`imagePullSecrets`|The names of the kubernetes secrets with credentials to login to a registry.|`[]`|
|`nameOverride`||`""`|
|`fullnameOverride`||`""`|
|`yarp.controllerClass`|The name for `IngressClass`. You can use `ingressClassName` for your `Ingress` to bind the ingress resource to the `NetCorePal Ingress Controller`|use `.Release.Name`|
|`yarp.isDefaultClass`|Set annotations `ingressclass.kubernetes.io/is-default-class: true`  for `ingressclass` if set to `true`, more info see <https://kubernetes.io/docs/concepts/services-networking/ingress/#default-ingress-class>.|`false`|
|`yarp.serverCertificates`||`false`|
|`yarp.defaultSslCertificateSecretNamespace`||use `.Release.Namespace`|
|`yarp.defaultSslCertificateSecretName`||`""`|
|`netcorepal.useMetricServer`|use [prometheus-net](https://github.com/prometheus-net/prometheus-net) to expose default metrics, so you can see the metrics in your grafana dashboard. use path `/metrics` to get metrics data.|`false`|
|`netcorepal.useHttpMetrics`|use [prometheus-net] exposes HTTP reuqest metrics if set to `true`.|`false`|
|`netcorepal.useYarpProxyStateUI`|Dashboard for Ingress Controller, use `https://your-ingress-controller-domain/_state` to open the dashboard, see <https://github.com/netcorepal/netcorepal-extensions-yarp>.|`true`|
|`serviceAccount.create`|Specifies whether a service account should be created.|`true`|
|`serviceAccount.annotations`|Annotations to add to the service account.|`{}`|
|`serviceAccount.name`|The name of the service account to use.If not set and create is true, a name is generated using the fullname template.|`""`|
|`podAnnotations`|The Annotations for Ingress Controller Pod.|`{}`|
|`podSecurityContext`|The SecurityContext for Ingress Controller Pod.|`{}`|
|`securityContext`|The SecurityContext for Ingress Controller Containers.|`{}`|
|`service.type`|Service Type.|`LoadBalancer`|
|`service.port`|Service Port.|`80`|
|`resources`|The cpu and memory resources requests and  limits.|`{}`|
|`autoscaling.enabled`||`false`|
|`autoscaling.minReplicas`||`1`|
|`autoscaling.maxReplicas`||`100`|
|`autoscaling.targetCPUUtilizationPercentage`||`80`|
|`nodeSelector`|The node selector for Ingress Controller Pod.|`{}`|
|`tolerations`|The tolerations list.|`[]`|
|`affinity`|The affinity.|`{}`|
|`serviceMonitor.selfMonitor`|Use ServiceMonitor for prometheus operator if set to `true`.|`false`|
