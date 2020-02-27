# istio-operator

```shell script
# apply
istioctl manifest apply \
-f examples/default.yaml \
--set installPackagePath=$PWD/charts

# generate
istioctl manifest generate \
-f examples/default.yaml \
--set installPackagePath=$PWD/charts \
> generate-default.yaml

# generate后kubectl apply测试部署失败
```

## Envoy日志
[istio 数据面调试指南](https://imfox.io/2020/02/12/istio-debug-with-envoy-log/)
```
kubectl -n istio-system edit cm istio
......
# Set accessLogFile to empty string to disable access log.
accessLogFile: "/dev/stdout" # 开启日志

accessLogEncoding: 'JSON' # 默认日志是单行格式， 可选设置为 JSON
......
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
