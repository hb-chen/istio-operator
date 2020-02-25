# istio-operator

```shell script
istioctl manifest generate \
-f examples/default.yaml \
--set installPackagePath=$PWD/charts \
> generate-default.yaml
```

```shell script
istioctl manifest generate \
--set profile=default \
--set installPackagePath=$PWD/charts \
--set values.global.hub=dockerhub.azk8s.cn/istio \
--set values.prometheus.hub=dockerhub.azk8s.cn/prom \
--set values.prometheus.ingress.enabled=true \
--set values.grafana.enabled=true \
--set values.grafana.ingress.enabled=true \
--set values.tracing.enabled=true \
--set values.tracing.ingress.enabled=true \
--set values.tracing.ingress.hosts[0]=tracing.local \
--set values.kiali.enabled=true \
--set values.kiali.ingress.enabled=true \
--set values.kiali.hub=quay.azk8s.cn/kiali \
--set values.kiali.createDemoSecret=true \
> generate-default.yaml

istioctl manifest generate \
--set profile=default \
--set installPackagePath=$PWD/charts \
--set values.global.hub=gcr.mirrors.ustc.edu.cn/istio-testing \
--set values.prometheus.hub=docker.mirrors.ustc.edu.cn/prom \
--set values.prometheus.ingress.enabled=true \
--set values.grafana.enabled=true \
--set values.grafana.ingress.enabled=true \
--set values.tracing.enabled=true \
--set values.tracing.ingress.enabled=true \
--set values.tracing.ingress.hosts[0]=tracing.local \
--set values.kiali.enabled=true \
--set values.kiali.ingress.enabled=true \
--set values.kiali.hub=quay.azk8s.cn/kiali \
--set values.kiali.createDemoSecret=true \
> generate-default.yaml


istioctl manifest generate \
--set profile=demo \
--set installPackagePath=$PWD/charts \
--set values.global.hub=gcr.azk8s.cn/istio-testing \
--set values.prometheus.hub=dockerhub.azk8s.cn/prom \
--set values.prometheus.ingress.enabled=true \
--set values.grafana.enabled=true \
--set values.grafana.ingress.enabled=true \
--set values.tracing.enabled=true \
--set values.tracing.ingress.enabled=true \
--set values.tracing.ingress.hosts[0]=tracing.local \
--set values.kiali.enabled=true \
--set values.kiali.ingress.enabled=true \
--set values.kiali.hub=quay.azk8s.cn/kiali \
--set values.kiali.createDemoSecret=true \
> generate-demo.yaml
```
