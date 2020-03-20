# istio-operator

在官方release版本基础上自定义charts，以及自己部署的examples示例

- charts
    - [ ] `ingress`中有`addongateway`，这样不依赖`ingress`，但都是`https`，可以增加`http`发布测试
    - ingress    
        - [x] `grafana`
        - [x] `kiali`
        - [x] `tracing`
    - contextPath
        - `prometheus`和`grafana`的`deployment`未支持路由的子路径，PR[istio#21383](https://github.com/istio/istio/pull/21383)
            - *或者通过`ingress`做`rewrite`，`VirtualService`也支持`rewrite`*
        - `kiali`依赖的`prometheus`增加`prometheus.contextPath`支持，PR[istio#21668](https://github.com/istio/istio/pull/21668)
- examples
    - `default.yaml`
        - `grafana`的`datasources`需要根据`prometheus`的`contextPath`配置，需要自定义，其他配置正常
- 国内镜像源
    - Istio `release`版本镜像在`docker.io/istio`，而`dev`版本镜像在`gcr.io/istio-testing`，注意根据自己的需求选择
    - [mirror.azure.cn](http://mirror.azure.cn/help/)
        - gcr.azk8s.cn
        - dockerhub.azk8s.cn
        - quay.azk8s.cn
    - [mirrors.ustc.edu.cn](http://mirrors.ustc.edu.cn/)

## istioctl部署

**`manifest apply`直接部署**
```shell script
# -f 自定义配置
# --set install_package_path 自定义charts
istioctl manifest apply \
-f examples/default.yaml \
--set profile=$PWD/profiles/default.yaml \
--set install_package_path=$PWD/charts
```

**`manifest generate`后`kubectl apply`部署**

> 测试部署容易出问题，还是推荐`manifest apply`

```shell script
istioctl manifest generate \
-f examples/default.yaml \
--set profile=$PWD/profiles/default.yaml \
--set install_package_path=$PWD/charts \
> generate-default.yaml

```

**升级**
```
# 环境检查
istioctl manifest versions
kubectl config view

# 不支持--set，所以install_package_path要在config file中指定
istioctl upgrade -f `<your-custom-configuration-file>`
```

**升级Sidecar**
```
Upgrade submitted. Please use `istioctl version` to check the current versions.
To upgrade the Istio data plane, you will need to re-inject it.
If you’re using automatic sidecar injection, you can upgrade the sidecar by doing a rolling update for all the pods:
    kubectl rollout restart deployment --namespace <namespace with auto injection>
If you’re using manual injection, you can upgrade the sidecar by executing:
    kubectl apply -f < (istioctl kube-inject -f <original application deployment yaml>)
```

**卸载**
```
istioctl manifest generate \
-f examples/default.yaml \
--set installPackagePath=$PWD/charts | kubectl delete -f -
```

## Envoy日志
[istio 数据面调试指南](https://imfox.io/2020/02/12/istio-debug-with-envoy-log/)
[Envoy RESPONSE_FLAGS说明](https://www.envoyproxy.io/docs/envoy/latest/configuration/observability/access_log)
```
kubectl -n istio-system edit cm istio
......
# Set accessLogFile to empty string to disable access log.
accessLogFile: "/dev/stdout" # 开启日志

accessLogEncoding: 'JSON' # 默认日志是单行格式， 可选设置为 JSON
......
```

## --set指定配置

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
```
