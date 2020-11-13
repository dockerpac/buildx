# Register buildx as default build
```shell
docker buildx install
```
# Build a custom buildx image with DTR certs
```shell
docker build -t dockerpac/buildx:amberjack -f Dockerfile.buildx --build-arg DTR_URL=dtr.pac-amberjack.dockerps.io --push .
```

# Create a buildkit runner in buildx namespace

```shell
docker buildx rm kube-amberjack
docker buildx create --name kube-amberjack --driver kubernetes --buildkitd-flags '--debug --allow-insecure-entitlement security.insecure' --driver-opt image=dockerpac/buildx:amberjack,namespace=buildx,replicas=5,rootless=true --use
docker buildx inspect --bootstrap
```

# Create an alpine image and push to DTR
```shell
docker login dtr.pac-amberjack.dockerps.io
docker build -t dtr.pac-amberjack.dockerps.io/admin/alpine:latest -f Dockerfile.alpine --push .
```
