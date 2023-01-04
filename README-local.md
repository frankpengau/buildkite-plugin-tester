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

## Checking Docker Image History (Layers?)
```
➜  buildkite-plugin-tester git:(bats-1-8-2-no-faccessat-multi-arch-sha-docker-buildx) docker image history frankpengau/buildkite-plugin-tester:bats-1-8-2-no-faccessat2-multi-arch-sha-docker-buildx
IMAGE          CREATED          CREATED BY                                      SIZE      COMMENT
3e61da64bdc3   32 minutes ago   CMD ["bats" "tests/"]                           0B        buildkit.dockerfile.v0
<missing>      32 minutes ago   ENTRYPOINT []                                   0B        buildkit.dockerfile.v0
<missing>      32 minutes ago   WORKDIR /plugin                                 0B        buildkit.dockerfile.v0
<missing>      32 minutes ago   ENV BATS_PLUGIN_PATH=/usr/local/lib/bats        0B        buildkit.dockerfile.v0
<missing>      32 minutes ago   RUN /bin/sh -c if [[ -e /bin/bash ]]; then e…   0B        buildkit.dockerfile.v0
<missing>      32 minutes ago   RUN /bin/sh -c mkdir -p /usr/local/lib/bats/…   152kB     buildkit.dockerfile.v0
<missing>      32 minutes ago   RUN /bin/sh -c mkdir -p /usr/local/lib/bats/…   24.8kB    buildkit.dockerfile.v0
<missing>      32 minutes ago   RUN /bin/sh -c mkdir -p /usr/local/lib/bats/…   104kB     buildkit.dockerfile.v0
<missing>      32 minutes ago   RUN /bin/sh -c mkdir -p /usr/local/lib/bats/…   34.7kB    buildkit.dockerfile.v0
<missing>      32 minutes ago   RUN /bin/sh -c apk --no-cache add ncurses cu…   2.55MB    buildkit.dockerfile.v0
<missing>      2 months ago     ENTRYPOINT ["/tini" "--" "bash" "bats"]         0B        buildkit.dockerfile.v0
<missing>      2 months ago     WORKDIR /code/                                  0B        buildkit.dockerfile.v0
<missing>      2 months ago     RUN |2 TINI_VERSION=v0.19.0 TARGETPLATFORM=l…   0B        buildkit.dockerfile.v0
<missing>      2 months ago     COPY . /opt/bats/ # buildkit                    486kB     buildkit.dockerfile.v0
<missing>      2 months ago     RUN |2 TINI_VERSION=v0.19.0 TARGETPLATFORM=l…   18B       buildkit.dockerfile.v0
<missing>      2 months ago     RUN |2 TINI_VERSION=v0.19.0 TARGETPLATFORM=l…   34.7MB    buildkit.dockerfile.v0
<missing>      2 months ago     RUN |2 TINI_VERSION=v0.19.0 TARGETPLATFORM=l…   2.34MB    buildkit.dockerfile.v0
<missing>      2 months ago     COPY ./docker /tmp/docker # buildkit            7.35kB    buildkit.dockerfile.v0
<missing>      2 months ago     ARG TARGETPLATFORM                              0B        buildkit.dockerfile.v0
<missing>      2 months ago     ARG TINI_VERSION=v0.19.0                        0B        buildkit.dockerfile.v0
<missing>      20 months ago    /bin/sh -c #(nop)  CMD ["bash"]                 0B        
<missing>      20 months ago    /bin/sh -c #(nop)  ENTRYPOINT ["docker-entry…   0B        
<missing>      20 months ago    /bin/sh -c #(nop) COPY file:651b3bebeba8be91…   212B      
<missing>      20 months ago    /bin/sh -c set -eux;   apk add --no-cache --…   8.01MB    
<missing>      20 months ago    /bin/sh -c #(nop)  ENV _BASH_LATEST_PATCH=4     0B        
<missing>      20 months ago    /bin/sh -c #(nop)  ENV _BASH_BASELINE=5.1       0B        
<missing>      20 months ago    /bin/sh -c #(nop)  ENV _BASH_VERSION=5.1.4      0B        
<missing>      20 months ago    /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B        
<missing>      20 months ago    /bin/sh -c #(nop) ADD file:3db1e10ac5ebf1cb3…   5.32MB    
➜  buildkite-plugin-tester git:(bats-1-8-2-no-faccessat-multi-arch-sha-docker-buildx) docker image history frankpengau/buildkite-plugin-tester:bats-1-8-2-no-faccessat2-multi-arch-sha-docker-build 
IMAGE          CREATED          CREATED BY                                      SIZE      COMMENT
21af5edfbd81   39 minutes ago   CMD ["bats" "tests/"]                           0B        buildkit.dockerfile.v0
<missing>      39 minutes ago   ENTRYPOINT []                                   0B        buildkit.dockerfile.v0
<missing>      39 minutes ago   WORKDIR /plugin                                 0B        buildkit.dockerfile.v0
<missing>      39 minutes ago   ENV BATS_PLUGIN_PATH=/usr/local/lib/bats        0B        buildkit.dockerfile.v0
<missing>      39 minutes ago   RUN /bin/sh -c if [[ -e /bin/bash ]]; then e…   0B        buildkit.dockerfile.v0
<missing>      39 minutes ago   RUN /bin/sh -c mkdir -p /usr/local/lib/bats/…   152kB     buildkit.dockerfile.v0
<missing>      39 minutes ago   RUN /bin/sh -c mkdir -p /usr/local/lib/bats/…   24.8kB    buildkit.dockerfile.v0
<missing>      39 minutes ago   RUN /bin/sh -c mkdir -p /usr/local/lib/bats/…   104kB     buildkit.dockerfile.v0
<missing>      39 minutes ago   RUN /bin/sh -c mkdir -p /usr/local/lib/bats/…   34.7kB    buildkit.dockerfile.v0
<missing>      39 minutes ago   RUN /bin/sh -c apk --no-cache add ncurses cu…   2.55MB    buildkit.dockerfile.v0
<missing>      2 months ago     ENTRYPOINT ["/tini" "--" "bash" "bats"]         0B        buildkit.dockerfile.v0
<missing>      2 months ago     WORKDIR /code/                                  0B        buildkit.dockerfile.v0
<missing>      2 months ago     RUN |2 TINI_VERSION=v0.19.0 TARGETPLATFORM=l…   0B        buildkit.dockerfile.v0
<missing>      2 months ago     COPY . /opt/bats/ # buildkit                    486kB     buildkit.dockerfile.v0
<missing>      2 months ago     RUN |2 TINI_VERSION=v0.19.0 TARGETPLATFORM=l…   18B       buildkit.dockerfile.v0
<missing>      2 months ago     RUN |2 TINI_VERSION=v0.19.0 TARGETPLATFORM=l…   34.7MB    buildkit.dockerfile.v0
<missing>      2 months ago     RUN |2 TINI_VERSION=v0.19.0 TARGETPLATFORM=l…   2.34MB    buildkit.dockerfile.v0
<missing>      2 months ago     COPY ./docker /tmp/docker # buildkit            7.35kB    buildkit.dockerfile.v0
<missing>      2 months ago     ARG TARGETPLATFORM                              0B        buildkit.dockerfile.v0
<missing>      2 months ago     ARG TINI_VERSION=v0.19.0                        0B        buildkit.dockerfile.v0
<missing>      20 months ago    /bin/sh -c #(nop)  CMD ["bash"]                 0B        
<missing>      20 months ago    /bin/sh -c #(nop)  ENTRYPOINT ["docker-entry…   0B        
<missing>      20 months ago    /bin/sh -c #(nop) COPY file:651b3bebeba8be91…   212B      
<missing>      20 months ago    /bin/sh -c set -eux;   apk add --no-cache --…   8.01MB    
<missing>      20 months ago    /bin/sh -c #(nop)  ENV _BASH_LATEST_PATCH=4     0B        
<missing>      20 months ago    /bin/sh -c #(nop)  ENV _BASH_BASELINE=5.1       0B        
<missing>      20 months ago    /bin/sh -c #(nop)  ENV _BASH_VERSION=5.1.4      0B        
<missing>      20 months ago    /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B        
<missing>      20 months ago    /bin/sh -c #(nop) ADD file:3db1e10ac5ebf1cb3…   5.32MB    
➜  buildkite-plugin-tester git:(bats-1-8-2-no-faccessat-multi-arch-sha-docker-buildx) 
```


# Resources
- Docker Buildx Docs: https://docs.docker.com/engine/reference/commandline/buildx_build/
- Docker Buildkit Docs: https://docs.docker.com/build/buildkit/
    - LLB: Low Level Build
    - Might relate to the buildkit error we got:
        ```
        failed to solve: rpc error: code = Unknown desc = failed to solve with frontend dockerfile.v0: failed to create LLB definition: rpc error: code = Unknown desc = error getting credentials - err: exit status 1, out: ``
        ```
    - Dockerfile Frontend
- 
