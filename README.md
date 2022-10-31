# Download Istio
If you want to download the latest version:
```bash
curl -L https://raw.githubusercontent.com/istio/istio/master/release/downloadIstioCandidate.sh | sh -
```

If you want to download a specific version, replace ````<YOUR RELEASE HERE>```` with your preferred version. Check the latest releases here:
https://github.com/istio/istio/releases
```bash
curl -L https://raw.githubusercontent.com/istio/istio/master/release/downloadIstioCandidate.sh | ISTIO_VERSION=<YOUR RELEASE HERE> TARGET_ARCH=x86_64 sh -.
```

# Install Custom Resource Definitions & Default Service Account/Roles/Bindings
Replace ````<YOUR ISTIO VERSION>```` with your istio version.
```bash
kubectl create namespace istio-system
helm install istio-base ./istio-<YOUR ISTIO VERSION>/manifests/charts/base -n istio-system
```

# Install Istio Discovery Service
If you want to customize your Istio Discovery configuration, check this link:
https://github.com/istio/istio/blob/master/manifests/charts/istio-control/istio-discovery/values.yaml
```bash
helm install istiod ./istio-<YOUR ISTIO VERSION>/manifests/charts/istio-control/istio-discovery -n istio-system --wait
```

# Install Ingress Gateway
I recommend you use a namespace othern than istio-system to install your Ingress Gateway. For this example I will use the "gateway" namespace.
If you want to customize your Istio Ingress Gateway configuration, check this link:
https://github.com/istio/istio/blob/master/manifests/charts/gateways/istio-ingress/values.yaml
```bash
kubectl create namespace gateway
kubectl label namespace gateway istio-injection=enabled
helm install istio-ingress ./istio-<YOUR ISTIO VERSION>/manifests/charts/gateways/istio-ingress -n gateway -f ingressgateway-values.yaml --wait
```

# Deploy Gateway
You need to install a Istio gateway for your applications to be exposed through that gateway.
In gateway.yaml there's an example gateway you can install locally.
```bash
kubectl apply -f gateway.yaml
```

# Deploy Telemetry
Deploy Telemetry to be able to see your Ingress Gateway logs.
```bash
kubectl apply -f telemetry.yaml
```