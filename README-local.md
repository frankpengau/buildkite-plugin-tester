# Notes

## Docker Build (build image)
```
docker build -t frankpengau/buildkite-plugin-tester:exampletag .
```

E.g. `docker build -t frankpengau/buildkite-plugin-tester:bats-1-8-2-no-faccessat2-multi-arch-sha-docker-build .`

## Docker Push (push image to docker hub)

**Ensure the docker image repo (e.g. frankpengau/buildkite-plugin-tester) exists first!!!**

### Need to login first
```
docker login -u frankpengau
```

Type password afterwards

Ensure login is successful!

### Docker Push Command

```
docker push frankpengau/buildkite-plugin-tester:exampletag
```

E.g.
- `docker push frankpengau/buildkite-plugin-tester:bats-1-8-2-no-faccessat2-multi-arch-sha-docker-build`
- `docker push frankpengau/buildkite-plugin-tester:bats-1-8-2-no-faccessat2-multi-arch-sha-docker-buildx`


## Docker Buildx (New Buildkit - build image with multi-architecture base image in Dockerfile)
```
docker buildx build -t frankpengau/buildkite-plugin-tester:exampletag .
```

E.g. 
- `docker buildx build -t frankpengau/buildkite-plugin-tester:bats-1-8-2-no-faccessat2-multi-arch-sha-docker-buildx .`
- No Cache, to avoid getting the cached version from docker build previously: `docker buildx build --no-cache -t frankpengau/buildkite-plugin-tester:bats-1-8-2-no-faccessat2-multi-arch-sha-docker-buildx .`


## Getting the manifest to see the SHA for the multi-architecture image
```
docker buildx imagetools inspect bats/bats:1.8.2-no-faccessat2
```

In this example, the very first SHA is the multi-architecture image SHA.
E.g. For bats/bats:1.8.2-no-faccessat2, the multi-architecture image SHA: `ed084f4b241c7e43422ff0a1d624a3a9609ef804ac8953449d2da63d6b8246e0`.

```
➜  buildkite-plugin-tester git:(bats-1-8-2-no-faccessat-multi-arch-sha-docker-buildx) ✗ docker buildx imagetools inspect bats/bats:1.8.2-no-faccessat2
Name:      docker.io/bats/bats:1.8.2-no-faccessat2
MediaType: application/vnd.docker.distribution.manifest.list.v2+json
Digest:    sha256:ed084f4b241c7e43422ff0a1d624a3a9609ef804ac8953449d2da63d6b8246e0
           
Manifests: 
  Name:      docker.io/bats/bats:1.8.2-no-faccessat2@sha256:ab8b147d6fd604a25872580db473a61d446370af00addfe0a0d8b4394eae0f6b
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/amd64
             
  Name:      docker.io/bats/bats:1.8.2-no-faccessat2@sha256:d78c8e0131d663478b56becb89d424064d32235e38806ec9b418e3fcb19cc20c
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/arm64
             
  Name:      docker.io/bats/bats:1.8.2-no-faccessat2@sha256:6f6141db058f3558077d9d6d3b1198feac62d0082a04021f1ec03d095ede28a1
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/ppc64le
             
  Name:      docker.io/bats/bats:1.8.2-no-faccessat2@sha256:2377ef3ee5c35745dad4b5b7e81c26a0da3dddc1ea368aa86dcf244d6d14dc9a
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/s390x
             
  Name:      docker.io/bats/bats:1.8.2-no-faccessat2@sha256:9d8278da26b2c38c4f3284c753a9575ba0260ae76a41816f83316bc8c4df5d3a
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/386
             
  Name:      docker.io/bats/bats:1.8.2-no-faccessat2@sha256:45277f5314b10ac92582c9d97a545134f8f7cbb1dafa4a8bc511ed22d50d7243
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/arm/v7
             
  Name:      docker.io/bats/bats:1.8.2-no-faccessat2@sha256:b7119bec8bd3346aa44d721c705616f3e98064c69b18a36f25e38403d3655db7
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/arm/v6
```

# Resources
- Docker Buildx Docs: https://docs.docker.com/engine/reference/commandline/buildx_build/

